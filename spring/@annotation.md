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