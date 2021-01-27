# 리액트를 익히기 위한 학습
 - nodejs설치, atom툴로 학습
 - 간단하게 CRUD하기
 - 이벤트 함수 및 기능 만들기

# Object1 > Object2 부모 자식 구조에서 자식이 부모의 기능을 공유 할 때
 ### 자식의 부모의 기능을 쓰게하려면 단순하게 부모의 기능을 넘겨주기만 하면 된다.
 
 - 부모가 자식에게 props, state, data뿐만 아니라 function기능로 함께 내릴 수 있다.
```
 ex)
	class Parens extends Content.Content {
		constructor(props) {
			super(props);
			this.state = {action: "Action", expand: true};
			this.setSelectedAction = this.setSelectedAction.bind(this);
		}

		setSelectedAction(event) {
			this.setState({action: event.target.value})
		}

		render() {
			const selectMenus = (
				<div className="mg10t">
					<select defaultValue={this.state.action} onChange={this.setSelectedAction}>
						<option value="Modify Object">Modify Object</option>
					</select>
				</div>
			)
			return (
				<div>
					<Child data={...data} selectMenus={selectMenus}/>
				</div>
			);
		}
	}
```
 
# Object1 === Object2 동일선상에서 서로 기능을 공유를 할 때
### 동일선상에 Object는 상위에서 공유를 시켜주는 방법으로 하면 된다.
 - ref={이 class가 담길변수명} 아래 예시와 같이 ref는 리액스에서 제공되는 기능 중 하나 
```
	ex)
	class Test extends React... {
		constructor(props) {
			super(props);
			this.refObject = React.createRef();
		}
		render() {
			return (
				<>
					<Object1 ref={el => this.refObject = el}/>
					
					{/* 이렇게 사용하면 object1에서 binding된 기능을 object2에서 사용할 수 있다.*/}
					<Object2 object1Current={this.refObject.current}/> 
				</>
			);
		}
	}
	export {Test};
```

# 제 3의 Object가 다른 Object자원을 공유 할 때
 - common.js에 구현 된 기능과 유사함
```
 	ex)
	{/* 외부에서 사용할 수 있도록 export로 메소드를 구현해준다. */}
	export function test(param1, param2) {
		sumeting logic...
	};
	
	class Test extends React... {
		constructor(props) {
			super(props);
			this.refObject = React.createRef();
			this.test = test.bind(this);  << 사용할 수 있게 바인딩 해준다.
		}
		render() {
			return (
				<>
					<Object1 ref={component => Common.setComponent(component)}/>
					
					{/* 이렇게 사용하면 object1에서 binding된 기능을 object2에서 사용할 수 있다.*/}
					<Object2 object1Current={this.refObject.current}/> 
				</>
			);
		}
	}
	export {Test};
```
### 그리고 나서 Common부분에 구현된 로직은 아래와 같다.
```
	const globalComponents = {};
	const Common = {
		setComponent: (component) => {
			globalComponents.test = component
		},

		test: (param1, param2) => {
			globalComponents.test.test(param1, param2)
		}
	}
	
	이후 제3자에서는 common.js를 import하고
	Common.test(param1, param2); 호출하게되면 Test클래스의 test(param1, param2) 메소드를 사용할 수 있는 것이다.
}
```
 
