### Spring Data JPA를 쓰면서 동적 페이징처리가 매우 쉬워졌다.

바로 PageRequest 객체가 자동으로 limit, offset으로 페이징하던 이전 쿼리를 자동으로 생성해주기 때문이다.

### Page와 Slice

PageRequest 객체를 통해 페이징을 할 때 반환형으로 Page와 Slice를 사용한다.두 객체의 결과물과 성능은 어떤 차이가 있는지 확인해보자

### Repository Interface

```java
//MemberRepository

...

    Page<Member> findPageBy(Pageable pageable);
    Slice<Member> findSliceBy(Pageable pageable);

...

```

위와 같이 Page와 Slice를 반환해주는 레포지토리를 만들고

```java
    @Test
    public void paging() throws Exception {
        //given
        memberRepository.save(new Member("member1", 10));
        memberRepository.save(new Member("member2", 10));
        memberRepository.save(new Member("member3", 10));
        memberRepository.save(new Member("member4", 10));
        memberRepository.save(new Member("member5", 10));

        PageRequest pageRequest = PageRequest.of(1, 2, Sort.by(Sort.Direction.ASC, "age"));

        //when
        Page<Member> page = memberRepository.findPageBy(pageRequest);
        Slice<Member> slice = memberRepository.findSliceBy(pageRequest);

        //then
        ...
    }

```

테스트를 돌려보면

Page의 경우:

> select   
    member0_.member_id as member_i1_0_,   
    member0_.age as age2_0_,   
    member0_.team_id as team_id4_0_,   
    member0_.username as username3_0_    
from   
    member member0_    
order by   
    member0_.age asc limit ? offset ?   
> 

> select   
    count(member0_.member_id) as col_0_0_    
from   
    member member0_   
> 

```java
select    
member0_.member_id as member_i1_0_,    
member0_.age as age2_0_,    
member0_.team_id as team_id4_0_,    
member0_.username as username3_0_    
from    
member member0_    
order by    
member0_.age asc limit 2 offset 2;
```

Slice의 경우:

> select   
    member0_.member_id as member_i1_0_,   
    member0_.age as age2_0_,   
    member0_.team_id as team_id4_0_,   
    member0_.username as username3_0_    
from   
    member member0_    
order by   
    member0_.age asc limit ? offset ?   
> 

```
select    
member0_.member_id as member_i1_0_,    
member0_.age as age2_0_,    
member0_.team_id as team_id4_0_,    
member0_.username as username3_0_    
from    
member member0_    
order by    
member0_.age asc limit 3 offset 2;
```

### 비교

page는 조회쿼리 이후 전체 데이터 갯수를 한번더 조회하는 카운트 쿼리가 실행된다.

slice는 limit(size)+1 된 값을 가져오는것을 확인할 수 있다.

### 정리

slice는 카운트쿼리가 나가지 않고 다음 slice가 존재하는지 여부만 확인할 수 있기때문에, 데이터 양이 많으면 많을수록 slice를 사용하는것이 성능상 유리하다.

page는 게시판과 같이 총 데이터 갯수가 필요한 환경에서,slice는 모바일과 같이 총 데이터 갯수가 필요없는 환경에서(무한스크롤 등),각각 필요한 용도에 알맞게 쓰면된다.
