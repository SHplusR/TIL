@RequestMapping

나는 주로 Controller에서 url을 묶을 때 사용한다.

예를 들어 

SampleController

```
@Controller
@RequestMapping("/sample")
@Log4j2
public class SampleController {

    @GetMapping("/ex1")
    public void ex1(){
        log.info("ex1.....");
    }
}
```

만약 내가 ex1의 페이지를 호출하고싶다면

localhost:8080/sample/ex1 을 호출하면 된다.

다만 이 경우에는 ex1.html이 꼭 templates의 sample패키지에 존재해야한다.

(존재하지 않으면 찾지 못해 에러뜬다)

다른 일부 프로젝트에서는 ex1()의 반환값을 String으로하여 

```
@GetMapping("/ex1")
public String ex1(){
    log.info("ex1.....");
    return "ex1";
}
```

이렇게도 하던데, 이렇게 하면 ex1.html이 templates의 어느 패키지에 있던 찾아서 화면을 불러줄수있다.

@Builder는 객체를 생성할때 사용하는 어노테이션이다.

만약 @Builder이 없을 경우, dto->entity 변환하는 작업이 있다고 했을때 아래와 같이 작성하게된다.

```
public static BoardEntity toSaveEntity(BoardDTO boardDTO){
    System.out.println("boardentity : tosaveentity");
    BoardEntity boardEntity = new BoardEntity();
    boardEntity.setBoardWriter(boardDTO.getBoardWriter());
    boardEntity.setBoardPass(boardDTO.getBoardPass());
    boardEntity.setBoardTitle(boardDTO.getBoardTitle());
    boardEntity.setBoardContents(boardDTO.getBoardContents());
    boardEntity.setBoardHits(0);
    boardEntity.setFileAttached(0);
    return boardEntity;
}
```

어쨌든 set을 하는건 똑같은데 column하나씩 설정을해야하고... 빌더패턴에도 맞지않다.

@Builder를 쓰면 아래와 같이 작성하면 된다.

```
    default Board dtoToEntity(BoardDTO dto){
        Member member = Member.builder().email(dto.getWriterEmail()).build();
        Board board = Board.builder()
                .bno(dto.getBno())
                .title(dto.getTitle())
                .content(dto.getContent())
                .writer(member)
                .build();
        return board;
    }
```

훨씬 간단하고 가독성도 좋다!


-----------------------------------------------------------------------

6/20

N:1(다대일관계) 일때 사용해야하는 어노테이션이 있다. 

바로 ManyToOne과 OneToMany이다.

 

예를 들어 회원과 게시글 / 게시글과 댓글의 관계를 생각해보면 아래와 같이 생각할 수 있다.

 

회원과 게시글
한 명의 회원은 여러 게시글을 작성할 수 있다.

 

게시글과 댓글
하나의 게시글에는 여러개의 댓글이 달릴 수 있다.

 

 

위 명제에서는 회원: 게시글 = 1:N으로 볼 수 있는데, 

이때 회원 입장에서는 @OneToMany, 게시글 입장에서는 @ManyToOne이다.

 

또 게시글: 댓글= 1:N으로 볼 수 있는데, 

이때 게시글 입장에서는 @OneToMany, 댓글 입장에서는 @ManyToOne이다.

 

내가 작성한 db구조를 보면 아래와 같다.(하이디sql에서는 다이얼그램을 제공안한다...)

 

member


board


'
reply


이름에서도 보이듯이

1. board 테이블은 member 테이블의 PK(email)를 FK(writer_email)로 가진다.

2. reply테이블은 board테이블의 PK(bno)를 FK(board_bno)로 가진다.

를 알 수 있다.

 

작성한 코드는 아래와 같다.

 

Board.java

    @ManyToOne(cascade = {CascadeType.ALL})
    private Member writer;
 

@ManyToOne은 Board가 Member에 대해 다(多) 관계를 가짐을 뜻한다. 

ManyToOne은 아래와 같은 속성을 가진다.

 


 

targetEntity() : 연결 대상인 엔터티 클래스로, 연결을 저장하는 필드 또는 속성의 유형이 기본값이다.
cascade() : 영속성 전이에 관련한 내용이다. 
fetch() : JPA에서 연관관계의 데이터를 어떻게 가져올것인지 정한다. Eager(즉시)과 Lazy(느린)가 있는데, 
즉시로딩은 불필요한 조인까지 처리해야하는 경우가 많기 때문에 보통 Lazy로 사용하는것 같다.
optional() : 연결이 선택 사항인지 여부이다. false로 설정하면 null이 아닌 관계가 항상 존재해야 하며, 기본값은 true다.
 

영속성 : 영원히 계속되는,유지된다는 뜻이다. 

영속성 전이라는건 어떤 엔티티에 영속성을 부여할때, 그 엔티티와 관련된 다른 엔티티에게도 영속성을 부여함을 뜻한다.

영속성 전이 설정에 대해서는 위에서 설명한것같이 cascade를 통해 변경 가능하다.

 

 


 

 

cascade타입에 대해 아래 블로그 글에서 아주 잘 설명되어있다.

 

CascadeType.ALL  : 모든 cascade를 적용한다.

CascadeType.RESIST – 엔티티를 생성하고, 연관 엔티티를 추가하였을 때 persist() 를 수행하면 연관 엔티티도 함께 persist()가 수행된다. 

CascadeType.DETACH – 부모 엔티티가 detach()를 수행하게 되면, 연관된 엔티티도 detach() 상태가 되어 변경사항이 반영되지 않는다. (detach : 분리)

CascadeType.MERGE – 트랜잭션이 종료되고 detach 상태에서 연관 엔티티를 추가하거나 변경된 이후에 부모 엔티티가 merge()를 수행하게 되면 변경사항이 적용된다.(연관 엔티티의 추가 및 수정 모두 반영됨)

CascadeType.REMOVE – 삭제 시 연관된 엔티티도 같이 삭제됨