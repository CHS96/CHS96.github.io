---
title:  "트랜잭션 고립화 레벨(Transaction Isolation Level)"
excerpt: "트랜잭션 고립화 레벨(Transaction Isolation Level)를 알아보자!"

categories:
  - 데이터베이스
  
last_modified_at: 2021-03-05T18:35:00
---

트랜잭션 고립화 레벨(Transaction Isolation Level)은 정규화, 인덱스, 트랜잭션 특징(ACID) 등 Database 면접 질문으로 자주 등장한다는 소리를 들었다. 따라서 이번 기회에 한번 공부 및 정리해보려고 한다.  

우선 Isolaion Level이 무엇인지 알아보기 전에 Isolation Level이 왜 필요한지부터 알아보자.  

Database에서 가장 중요한 것 중 하나는 **데이터의 무결성을 보장해줘야 한다.**  

![고립화레벨](https://user-images.githubusercontent.com/53072057/110067200-914a0800-7db6-11eb-8e03-f1e0563be3d7.JPG)  

만약, A가 DB에서 데이터를 사용하고 있다고 하자. 이때 B가 A가 사용하고 있는 데이터를 변경한다면 어떻게 될까? 데이터가 변경된다면 A는 원하는 데이터를 제대로 사용할 수 없다.  

즉, 이런 상황을 방지하기 위해 데이터의 무결성을 보장해줘야 하는 것이다.  

DB에서 수행되는 모든 작업은 **트랜잭션**이라고 한다.  

![고립화레벨1](https://user-images.githubusercontent.com/53072057/110067205-927b3500-7db6-11eb-8bca-253ab51af74f.JPG)  

트랜잭션은 무결성을 보장해주기 위해 **ACID(Atomicity, Consistency, Isolation, Durability)** 4가지 특성을 만족해야 한다.  

트랜잭션의 ACID에 대해 궁금하다면 다음 글을 참고하길 바란다.  

* <https://blog.naver.com/djena5078>  
[![고립화레벨2](https://user-images.githubusercontent.com/53072057/110067206-927b3500-7db6-11eb-9b4c-6608440638d7.JPG)](https://blog.naver.com/djena5078)  

즉, DB는 ACID 특징처럼 트랜잭션이 원자적이면서 독립적인 수행을 하도록 해줘야 하는데 이때 등장하는 개념이 Locking이다. 세마포어의 Lock을 생각하면 된다.  

![고립화레벨3](https://user-images.githubusercontent.com/53072057/110067208-9313cb80-7db6-11eb-8926-88d22870f5b8.JPG)  

무조건 강한 Locking 기법을 사용한다면 그만큼 많은 트랜잭션이 대기해야 하기 때문에 DB 성능이 떨어지게 되고 반대로 응답성을 높이기 위해 약한 Locking 기법을 사용한다면 데이터가 도중에 변경되는 등 잘못된 처리가 발생할 수 있게 된다.  

![고립화레벨4](https://user-images.githubusercontent.com/53072057/110067209-9313cb80-7db6-11eb-9b8f-a7c31e1bc875.JPG)  

따라서 우리는 최대한 효율적인 Locking 기법을 적용해야 한다. 이와 관련된 Locking 방법이 **고립화 단계(Isolation Level)**이다.  

Isolation Level는 대표적으로 4단계가 존재한다. 하나씩 살펴보도록 하자.  

<h2>[ Level 0 : Read Uncommitted ]</h2>  

* Select 문장을 수행하는 경우 해당 데이터에 Shared Lock이 걸리지 않은 가장 낮은 단계이다.  

* Read Uncommitted 의미 그대로 commit 되지 않은 데이터를 읽을 수 있다.  

* 즉, 트랜잭션이 끝나지 않은 상황에서 다른 트랜잭션이 접근 가능하기 때문에 데이터베이스의 일관성을 유지할 수 없다.  

![고립화레벨5](https://user-images.githubusercontent.com/53072057/110067211-93ac6200-7db6-11eb-8d46-d93cacf14dae.JPG)  

1. Transaction 1이 SELECT 쿼리를 수행하면 "아무개"가 조회된다.  

2. Transaction 1이 종료되지 않은 시점에서 Transaction 2가 "아무개"를 "개발자"로 변경하였다.  

3. 다시 한번 Transacction 1에서 SELECT 쿼리를 수행하게 되면 변경된 "개발자"가 조회된다.  


<h2>[ Level 1 : Read Committed ]</h2>  

* SQL Server가 Default로 사용하는 단계이다.  

* Select 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸린다.  

* Read Committed 의미 그대로 commit된 데이터만 접근이 가능하다.  

* Level 0 단계의 문제점을 개선하였지만 한 트랜잭션이 commit을 하기 전까지는 다른 트랜잭션은 접근할 수 없기 때문에 대기해야 하는 단점이 있다.  

![고립화레벨6](https://user-images.githubusercontent.com/53072057/110067213-93ac6200-7db6-11eb-97b1-579fe1c699ff.JPG)  

1. Transaction 2가 update 쿼리를 통해 name을 "개발자"에서 "아무개"로 변경하였다.  

2. Transaction 1에서 SELECT 쿼리를 수행하지만 해당 데이터가 아직 commit 되지 않았기 때문에 대기한다.  

3. Transaction 2에서 name 데이터를 commit 한다.  

4. Transaction 1에서 SELECT 쿼리를 다시 수행하면 "개발자"가 조회된다.  


<h2>[ Level 2 : Repatable Read ]</h2>  

* Level 2 단계에서 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸려 commit 전까지는 접근할 수 없었다. 하지만 그만큼 다른 트랜잭션은 대기해야 하는 단점이 있었다.  

* 이러한 문제점을 개선하기 위해 Level 3 단계는 트랜잭션이 SELECT 문장이 조회한 데이터를 제외한 해당 Row의 범위에는 다른 트랜잭션이 insert가 가능하다.  

* 제외한 범위에서 가능하기 때문에 당연히 트랜잭션이 최초 수행된 후 해당 범위 내에서는 조회한 데이터의 내용이 항상 동일함을 보장한다.  

![고립화레벨7](https://user-images.githubusercontent.com/53072057/110067214-9444f880-7db6-11eb-9144-827c8d003447.JPG)  

1. Transaction 1에서 SELECT 쿼리를 통해 "아무개"를 조회하였다.  

2. Transaction 1이 종료되지 않았지만 Transaction 2에서 user 테이블에 "동네개발자"라는 새로운 데이터를 insert 하였다.  

3. Transaction 2에서 "아무개"를 "개발자"로 업데이트하였다.  

4. Transaction 2에서 변경된 작업을 commit 한다.  

5. Transaction 1에서 SELECT 쿼리를 수행하면 insert는 반영되고 update는 반영되지 않은 "아무개"와 "동네개발자"가 조회된다.  


<h2>[ Level 3 : Serializable ]</h2>

* Index가 설정되어 있지 않다는 조건하에 모든 동작이 직렬화되어 동작한다.  

* 즉, Level 2 단계와 다르게 insert 작업을 수행하여도 동작하지 않는다.  

* 한마디로 트랜잭션의 모든 작업이 완료될 때까지는 다른 트랜잭션은 해당하는 영역의 데이터에는 Update 및 Insert 작업이 불가능하다.  

* 다만, Index가 설정되어 있다면 index 값을 통해 Update 및 Insert 작업을 수행할 수 있다.  

![고립화레벨8](https://user-images.githubusercontent.com/53072057/110067216-94dd8f00-7db6-11eb-95a9-a55c538e596c.JPG)  

1. Transaction 1에서 SELECT 쿼리를 통해 "아무개"를 조회하였다.  

2. Transaction 1이 종료되지 않았지만 Transaction 2에서 user 테이블에 "동네개발자"라는 새로운 데이터를 insert 하였다.  

3. Transaction 2에서 "아무개"를 "개발자"로 업데이트하였다.  

4. Transaction 2에서 변경된 작업을 commit 한다.  

5. Transaction 1에서 SELECT 쿼리를 수행하여도 update, insert가 수행되지 않은 상태인 "아무개"만 조회된다.  



마지막으로 Level에 따라 다음과 같은 **부작용(Sied Effect)이 존재**한다.  

![고립화레벨9](https://user-images.githubusercontent.com/53072057/110067217-94dd8f00-7db6-11eb-8595-021cda6f5820.JPG)  

<h2>[ Dirty Read ]</h2>  

* 잘못된 데이터를 읽는 것을 의미한다.  

* A Transaction 입장에서 아직 실행이 끝나지 않은 B Transaction에 의한 변경 사항을 보게 되는 경우  

* 만약 A가 변경된 데이터를 읽었는데 B가 변경한 데이터를 다시 롤백 해버리면 A는 Dirty 데이터를 가지게 된다.  

<h2>[ Non-Repeatable Read ]</h2>  

* A Transaction이 같은 질의를 계속해도 B Transaction이 commit하지 않았기 때문에 A는 계속 변경되기 전의 동일한 데이터만 읽어드리는 경우를 의미한다.  

* 즉, 다른 Transaction에 의한 변경 사항을 볼 수 없다.  

* 만약 보고 싶다면 Transaction을 새로 시작해야 한다.  

<h2>[ Phantom Read ]</h2>  

* 다른 Transaction에 의한 변경 사항으로 인해 현재 사용 중인 Transaction의 Where 절의 조건에 맞는 새로운 행이 생길 수 있는 상황을 의미한다.  

* 예를 들어 위의 Level 2의 REPEATABLE READ 예시를 보면 처음에는 "아무개", 1개의 데이터만 조회되었지만 이후에는 "아무개"와 "동네개발자", 2개의 데이터가 조회되었다.  

* 이처럼 Where 절의 조건에 맞는 새로운 행이 생길 수 있는 경우를 말한다.  


<h2>[ Reference ]</h2>  
* <https://goodgid.github.io/Transaction-Isolation-Level/>  

​


