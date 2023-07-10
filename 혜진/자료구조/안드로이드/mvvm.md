# ⭐mvvm

mvvm을 보기전에
## ❣️AAC(Android Architecture Components)
안드로이드 앱 아키텍처를 구축하기 위한 일련의 라이브러리 집합
안드로이드 앱의 개발을 더 쉽고 효율적으로 만들기 위해 Google에서 제공하는 공식 라이브러리이며, 일반적으로 MVVM (Model-View-ViewModel) 아키텍처를 구현하는 데 사용
AAC는 다음과 같은 주요 라이브러리로 구성되어 있습니다:

1. LiveData: 생명주기를 인식하는 관찰 가능한 데이터 홀더 클래스로, 데이터의 변경을 알림을 받을 수 있습니다. LiveData는 생명주기를 자동으로 처리하여 UI 업데이트를 쉽게 관리할 수 있습니다.

2. ViewModel: UI와 데이터 사이의 연결고리로 동작하는 클래스로, 액티비티나 프래그먼트와 같은 구성 요소의 생명주기와 독립적으로 데이터를 관리합니다. 액티비티나 프래그먼트 구성 변경 시에도 데이터를 보존할 수 있으며, 메모리 누수와 같은 문제를 방지합니다.

3. Room: SQLite를 기반으로 한 안드로이드용 ORM(Object-Relational Mapping) 라이브러리로, 로컬 데이터베이스에 대한 추상화 계층을 제공합니다. Room은 데이터베이스 액세스를 쉽게 관리하고 SQLite 작업을 간소화하여 안드로이드 앱의 데이터베이스 작업을 더 효율적으로 처리할 수 있도록 도와줍니다.

4. Paging: 대량의 데이터를 효율적으로 로딩하고 표시하기 위한 라이브러리로, RecyclerView와 함께 사용됩니다. 페이징 라이브러리를 사용하면 네트워크 요청을 최적화하고 데이터를 청크 단위로 로딩하여 부드러운 스크롤 및 페이징 경험을 제공할 수 있습니다.

5. Data Binding: XML 레이아웃 파일에서 데이터와 UI를 바인딩하여 자동으로 UI 업데이트를 처리하는 라이브러리입니다. 데이터 바인딩을 사용하면 UI 업데이트를 수동으로 처리하는 번거로움을 줄이고 코드의 가독성과 유지보수성을 향상시킬 수 있습니다.

AAC는 안드로이드 앱의 아키텍처 구성을 단순화하고 개발자가 더 쉽게 안정적이고 확장 가능한 앱을 작성할 수 있도록 돕습니다. 이러한 라이브러리들은 개별적으로 사용할 수도 있지만, 함께 사용하면 더 강력한 기능과 편리한 개발 경험을 제공합니다.



#### [예시]-mvvm 과 관련된것만

LiveData 예시:
```kotlin
val nameLiveData: MutableLiveData<String> = MutableLiveData()

// LiveData를 관찰하고 UI 업데이트 처리
nameLiveData.observe(this, Observer { name ->
    textView.text = name
})

// 데이터 변경 시 LiveData 값을 업데이트
nameLiveData.value = "John Doe"
```


ViewModel 예시:
```kotlin
class MyViewModel : ViewModel() {
    private val _count = MutableLiveData<Int>()
    val count: LiveData<Int> get() = _count

    init {
        _count.value = 0
    }

    fun incrementCount() {
        _count.value = _count.value?.plus(1)
    }
}

// ViewModel 초기화
val viewModel = ViewModelProvider(this).get(MyViewModel::class.java)

// LiveData를 관찰하고 UI 업데이트 처리
viewModel.count.observe(this, Observer { count ->
    textView.text = count.toString()
})

// 버튼 클릭 시 ViewModel에서 count 값 변경
button.setOnClickListener {
    viewModel.incrementCount()
}
```


Data Binding 예시:
```kotlin
// XML 레이아웃에서 변수로 사용할 User 객체
<data>
    <variable
        name="user"
        type="com.example.myapp.model.User" />
</data>

<!-- 데이터 바인딩을 사용하여 UI 업데이트 -->
<TextView
    android:text="@{user.name}"
    ... />

<TextView
    android:text="@{String.valueOf(user.age)}"
    ... />
```



#### 🍅참고 mvvm에서 ui를 업데이트하는 두가지 방법
Observer는 뷰모델의 데이터를 관찰하고 데이터가 변경될 때마다 알림을 받아 UI를 업데이트하는 방식입니다. 
Observer는 LiveData나 RxJava와 같은 라이브러리를 사용하여 구현할 수 있습니다. 예를 들어 LiveData를 사용하는 경우, 
LiveData 객체에 대한 Observer를 등록하고 데이터의 변경을 감지하여 UI를 업데이트할 수 있습니다. 아래는 간단한 예시입니다.
```kotiln
// LiveData와 Observer를 사용한 예시
val userLiveData: MutableLiveData<User> = MutableLiveData()

// Observer 등록
userLiveData.observe(this, Observer { user ->
    // 데이터 변경 시 UI 업데이트
    nameTextView.text = user.name
    ageTextView.text = user.age.toString()
})

// 데이터 변경 시 LiveData 값 업데이트
userLiveData.value = User("John Doe", 25)
```

반면에 Data Binding은 XML 레이아웃 파일에서 UI 컴포넌트와 데이터를 바인딩하여 데이터의 변경이 자동으로 UI에 반영되는 방식입니다. 
데이터 바인딩을 사용하면 XML 파일에서 직접 데이터와 UI를 연결할 수 있으며, 데이터가 변경되면 자동으로 UI가 업데이트됩니다. 
아래는 간단한 예시입니다.

```kotlin
// 데이터 바인딩을 사용한 예시
<data>
    <variable
        name="user"
        type="com.example.myapp.model.User" />
</data>

<TextView
    android:text="@{user.name}"
    ... />

<TextView
    android:text="@{String.valueOf(user.age)}"
    ... />
```

## 🦄mvvm

소프트웨어 아키텍처 패턴으로, 사용자 인터페이스 (UI) 로직과 비즈니스 로직을 분리하여 애플리케이션을 개발하는 방법입니다. 
MVVM은 모델(Model), 뷰(View), 뷰모델(ViewModel)로 구성됩니다.

모델(Model): 데이터와 비즈니스 로직을 담당합니다. 주로 데이터 소스 (로컬 DB, 원격 서버, 파일 등)와 통신하여 데이터를 가져오거나 조작하는 역할을 합니다.

뷰(View): 사용자에게 데이터를 시각적으로 표현하고 사용자 입력을 처리하는 부분입니다. 레이아웃, 위젯, 액티비티, 프래그먼트 등으로 구성됩니다.

뷰모델(ViewModel): 뷰와 모델 사이의 중간 매개체로서, 뷰와 데이터 간의 데이터 바인딩을 처리하고 뷰에 필요한 데이터와 동작을 제공합니다. 
뷰모델은 뷰에 대한 상태와 동작을 캡슐화하며, 뷰와 독립적으로 테스트 가능한 형태로 설계됩니다. 주로 LiveData나 RxJava와 같은 라이브러리를 사용하여 데이터 변경을 감지하고 뷰에 반영합니다.

MVVM 패턴의 핵심 아이디어는 데이터 바인딩입니다. 뷰와 뷰모델을 바인딩하여 데이터 변경 시 자동으로 업데이트되는 방식을 통해 뷰와 모델의 동기화를 유지합니다. 
이를 통해 UI 업데이트를 수동으로 처리하는 번거로움을 줄이고, 코드의 가독성과 유지보수성을 향상시킬 수 있습니다.

MVVM 패턴은 안드로이드 애플리케이션 개발에 많이 사용되며, Android Architecture Components (AAC) 라이브러리인 
ViewModel과 LiveData와 함께 사용되면 좀 더 효과적인 개발이 가능합니다. ViewModel은 액티비티나 프래그먼트의 구성 변경과 같은 상황에서도 데이터를 유지하고, 
LiveData는 데이터의 관찰과 뷰 갱신을 자동으로 처리할 수 있게 도와줍니다. 이러한 라이브러리들을 활용하여 MVVM 패턴을 구현하면 애플리케이션의 확장성과 유지보수성을 개선할 수 있습니다.


```kotlin
// 모델 클래스
data class User(val name: String, val age: Int)

// 뷰모델 클래스
class UserViewModel : ViewModel() {
    private val _user = MutableLiveData<User>()
    val user: LiveData<User> get() = _user

    fun fetchUser() {
        // 모델에서 데이터를 가져오는 비즈니스 로직
        val user = User("John Doe", 25)
        _user.value = user
    }
}

// 액티비티 클래스
class MainActivity : AppCompatActivity() {
    private lateinit var userViewModel: UserViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // 뷰모델 초기화
        userViewModel = ViewModelProvider(this).get(UserViewModel::class.java)

        // 뷰모델의 데이터 변경 감지 및 UI 업데이트
        userViewModel.user.observe(this, Observer { user ->
            updateUI(user)
        })

        // 사용자 정보 가져오기
        userViewModel.fetchUser()
    }

    private fun updateUI(user: User) {
        // UI 업데이트 로직
        nameTextView.text = user.name
        ageTextView.text = user.age.toString()
    }
}
```
위의 예시에서는 간단한 사용자 정보를 나타내는 User 클래스가 모델로 사용됩니다. 
UserViewModel 클래스는 ViewModel을 상속받아 작성되었습니다. UserViewModel 클래스에서는 사용자 데이터를 담는 _user 변수를 MutableLiveData(수정가능)로 선언하고, 
user 변수를 LiveData(수정불가)로 노출시켜 데이터 변경을 관찰할 수 있도록 합니다. fetchUser() 함수는 모델에서 데이터를 가져오는 비즈니스 로직을 담당하고, _user 값을 업데이트합니다.
