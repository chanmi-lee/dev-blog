---
layout: post
title: "HOC in React"
excerpt: "#React #HOC #HigherOrderComponent #DRY #DonotRepeatYourself"
categories: [글또, React]
share: true
comments: true
---

### Higher-Order Component?

HOC는 컴포넌트 로직을 사용하기 위한 리트의 고급 기법입니다.
HOC는 그 자체로 리액트 API는 아닙니다.
리액트의 구성 방식으로부터 파생된 패턴이라고 이해할 수 있습니다.

구체적으로, HOC는 컴포넌트를 파라미터로 받아 새로운 컴포넌트로 돌려주는 하나의 함수라고 이해할 수 있습니다.

```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

HOC를 통하여 컴포넌트에 특정 기능을 부여할 수 있습니다.
컴포넌트는 props를 변형하여 UI에 넘기는 것과 달리, HOC는 컴포넌트를 변형하여 이를 또 다른 컴포넌트에 넘겨줍니다.
HOC는 3rd party 라이브러리에서 흔히 찾아볼 수 있는데, 대표적인 예로는 Redux의 [connect](https://github.com/reduxjs/react-redux/blob/master/docs/api/connect.md#connect)와 Relay의 [createFragmentContainer](http://facebook.github.io/relay/docs/en/fragment-container.html)가 있습니다.

이제부터는, HOC가 유용한 이유와, 어떻게 HOC를 작성할 수 있는지 알아보고자 합니다.

---

### HOC의 유용성

> 이전에는 관심사의 분리(cross-cutting concerns)을 다루기 위한 방법으로 `mixins`을 사용하길 권장하였습니다. 
> 그러나, 이 방법이 다소 많은 문제점을 야기한다는 것을 알게 되었습니다.
> [링크](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html) 를 참고하시면, 왜 우리가 mixins로부터 벗어나야 하며 어떻게 기존의 컴포넌트를 변화시킬 수 있는지 이해하는데 도움이 될 것입니다.

리엑트에서 컴포넌트는 코드를 재사용하는 주요한 코드의 단위입니다.
그러나, 다음의 패턴들은 컴포넌트를 사용하는데 맞지 않음을 알 수 있습니다.

예를 들어, 댓글 목록을 렌더링하는데 외부 리소스를 subscribe하는 `CommentList`라는 컴포넌트가 있다고 합시다.

```
class CommentList extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        // "DataSource" is some global data source
        comments: DataSource.getComments()
      };
    }
  
    componentDidMount() {
      // Subscribe to changes
      DataSource.addChangeListener(this.handleChange);
    }
  
    componentWillUnmount() {
      // Clean up listener
      DataSource.removeChangeListener(this.handleChange);
    }
  
    handleChange() {
      // Update component state whenever the data source changes
      this.setState({
        comments: DataSource.getComments()
      });
    }
  
    render() {
      return (
        <div>
          {this.state.comments.map((comment) => (
            <Comment comment={comment} key={comment.id} />
          ))}
        </div>
      );
    }
  }
```

또한, 하나의 포스트를 위한 같은 패턴을 가진 `BlogPost`라는 이름 컴포넌트를 작성합니다.

```
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```

`CommentList`와 `BlogPost`는 다른 DataSource를 참조하는 메소드를 호출한다는 점과 다른 결과물을 렌더링한다는 점에서는 완전히 동일하지는 않습니만,
다음 공통점이 있습니다.

- 마운트될 때(componentDidMount), DataSource로의 change listener를 추가한다.
- 리스너 내부에, 데이터가 변경될 때마다 setState를 호출한다 (handleChange).
- 언마운트될 때(componentWillUnmount), change listener를 제거한다.


---

### HOC 작성하기

HOC의 이름을 만들 땐, `with____` 형식으로 짓습니다.

HOC의 원리는, 파라미터로 컴포넌트를 받아오고, 함수 내부에서 새 컴포넌트를 만든 다음에 해당 컴포넌트 안에서 파라미터로 받아온 컴포넌트를 렌더링하는 것입니다.
그리고 자신이 받아온 props들은 그대로 파라미터로 받아온 컴포넌트에게 다시 주입해주고, 필요에 따라 추가 props도 넣어줍니다.

우선 HOC의 틀을 작성해보겠습니다.


```
const CommentListWithSubscription = withSubscription(
    CommentList,
    (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
    BlogPost,
    (DataSource, props) => DataSource.getBlogPost(props.id)
);
```

`CommentList`와 `BlogPost`와 같이 DataSource를 참조하는 컴포넌트를 생성하는 함수를 작성하였습니.
이 함수는 참조하는 데이터를 props로 컴포넌트에 전달하며 이를 함수의 인자(argument)로 받습니다.
다

첫 번째 파라미터는 감싸진 컴포넌트(Wrapped component) 입니다.
두 번째 파라미터는 우리가 알고자 하는 데이터로 이를 DataSource로 받아와 props로 넘겨줍니다.

```
// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

> HOC는 다른 컴포넌트를 감싸는 컴포넌트일 뿐입니다.

HOC는 원래의 컴포넌트를 변형하지 않을 뿐 아니라, 상속을 사용하여 컴포넌트의 동작을 복제하지 않습니다.
오히려, HOC는 원본 컴포넌트를 컨테이너 컴포넌트와 같이 감싸는 컴포넌트일 뿐입니다. HOC는 순수 함수로써 아무런 사이드 이펙트가 없습니다.

그게 다입니다!

감싸진 컴포넌트는 컨테이너의 모든 props를 받아와, 새로운 props (위의 예시에서는 data)와 함께 새로운 결과를 렌더링합니다.
HOC는 어떻게 혹은 왜 데이터가 쓰이는지 관심이 없고, 감싸진 컴포넌트는 데이터가 어디로부터 오는지 직접 알 필요가 없습니다.

---

🔗 참조

📌 [Higher Order Component in React](https://reactjs.org/docs/higher-order-components.html)
