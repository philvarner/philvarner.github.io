### Migrating from Google Guava Optional

Prior to Java 8, many applications used the [http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Optional.html](com.google.common.base.Optional<T>) class in the Google Guava library to implement similar behavior to java.util.Optional.

| Guava | Java 8 |
| ---------------- | ---------------- |
| get() | get() |
| absent() | empty() |
| fromNullable(T) | ofNullable(T) |
| of(T) | of(T) | non-null reference |
| or(T) | orElseGet(T) |
| or(c.g.c.b.Supplier) | orElse(j.u.Supplier<? extends T>)|
| isPresent() | isPresent() |
| orNull() | orElseGet(null) | 
| transform(c.g.c.b.Function<? super T,V> function)) | map(j.u.Function<? super T,? extends U>) |
| none | ifPresent(Consumer<? super T>) |
| none | flatMap(j.u.Function<? super T, j.u.Optional<U>>) |
| none | filter(j.u.Predicate<? super T>) |
