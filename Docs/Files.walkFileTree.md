## Files.walkFileTree(Path start, FileVisitor visitor)

 - Java 7에서 지원하는 기능

```java
Files.walkFileTree(path, new FileVisitor<Path>() {
	@Override
    public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) throws IOException {
        return FileVisitResult.CONTINUE;
    }

    @Override
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
        return FileVisitResult.CONTINUE;
    }

    @Override
    public FileVisitResult visitFileFailed(Path file, IOException exc) throws IOException {
    }

    @Override
    public FileVisitResult postVisitDirectory(Path dir, IOException exc) throws IOException {
        return FileVisitResult.CONTINUE;
    }
});
```

|메소드|설명|
|---|---|
|preVisitDirectory|폴더에 접근 했을 때 동작|
|visitFile|파일에 접근 했을 때 동작|
|visitFileFailed|파일 접근에 실패 했을 때 동작|
|postVisitDirectory|폴더에서 떠날때 동작|

A/B/C depth로 구성되어 있는 directory A에 접근하게 되면

> preA -> preB -> preC -> postC -> postB -> postA

순으로 실행되게 된다.

<br/>

|리턴값|설명|
|---|---|
|FileVisitResult.Continue|계속 탐색|
|FileVisitResult.Terminate|탐색 종료|
|FileVisitResult.SKIP_SUBTREE|하위 디렉토리 무시|
|FileVisitResult.SKIP_SIBLINGS|형제 파일 무시|
