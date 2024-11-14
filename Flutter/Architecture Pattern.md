# 아키텍처 패턴(Architecture Pattern)
아키텍처 패턴이란 소프트웨어어 아키텍처의 공통적인 발생 문제에 대한 일반적인, 재사용 가능한 해결책을 의미합니다.
# 왜 쓸까?
코드 분리에 대한 기준이 명확하지 않아 생산성이 저하됨          
개발도중에도 기존 코드를 손대면 다른 코드에도 영향이 가게됨            
코드 리뷰를 할때에도 가독성과 재사용성이 현저히 떨어짐             
# MVC(모델-뷰-컨트롤러)
**Model**: 사용되는 데이터의 형태를 정의하고 데이터를 처리하는 부분                
**View**: 사용자에게 보여지는 부분(UI)            
**Controller**: 사용자의 입력을 받고 처리하는 부분
## 동작
`Controller`를 통해 사용자에게 입력받고 `Model`을 업데이트하면 `View`는 모델을 통해 `View`를 업데이트 한다.
## 특징
가장 단순한 패턴으로 구현이 쉽다                
`View`와 `Model`의 높은 의존도로 앱이 복잡해질 경우 유지보수가 어려워짐
# MVP(모델-뷰-프레젠터)
**Model**: 사용되는 데이터의 형태를 정의하고 데이터를 처리하는 부분                 
**View**: 사용자에게 보여지는 UI 부분             
**Presenter**: 사용자의 입력을 받고 처리하는 부분 
## 동작
`Controller`로 사용자 입력을 받는게 아니라 `View`가 사용자 입력을 받고 `Model` 업데이트 시키고 다시 `Presenter`가 `View` 업데이트를 한다
## 특징
`View`와 `Model`를 분리하여 의존성이 없다.            
`View`와 `Presenter`의 높은 의존도가 발생한다.           

`MVC에 비해 계층에 분리가 명확해졌으나 View가 많은 복잡한 프로젝트는 Present도 비례하여 많아져 복잡해짐`
# MVVM(모델-뷰-뷰모델)
**Model**: 사용되는 데이터의 형태를 정의하고 데이터를 처리하는 부분               
**View**: 사용자에게 보여지는 UI 부분             
**ViewModel**: View에서 필요한 데이터를 처리하는 부분
## 동작
`View`를 통해 사용자 입력을 받아 `ViewModel`로 보내면 `ViewModel`은 `Model`에 데이터를 요청하고 처리하고 저장한다. `View`는 직접 `ViewModel`과의 데이터 바인딩을 통해  `View`를 업데이트 한다. `ViewModel`은 `View`에 직접 데이터를 보내지 않는다.
## 특징
`View`와 `Model`에 의존성이 없음                 
여러개의 `View`가 하나의 `ViewModel`을 참조하는 방식으로 1 대 다 구조가 가능하다             
`ViewModel`의 설계가 잘 이루어져야 한다