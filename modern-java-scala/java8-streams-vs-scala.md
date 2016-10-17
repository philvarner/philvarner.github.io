
Scala's Lists support mostly the same API that the Java 8 Stream API supports

predicate of form s => s == "x"

| Item         | Java 8 Stream           | Scala   |
| ------------ | ----------------------- | ------- |
| Creation     | Collection#stream() or StreamSupport.stream(Iterable#spliterator(), boolean) | List(...), Nil, "A" :: Nil | 
| Map | .map(Function) [returns Stream] | .map(...) [returns List] |
| Filter | .filter(predicate) | list.filter(predicate) |
| Any Match    | anyMatch(Predicate)     | list.exists(s => s == "x") |
| All Match    | allMatch(Predicate)     | list.forall(s => s == "x") |
| Concatenation | Stream.concat(a,b) | List("A", "B") ::: List("C", "D") |
| Join to String | .map(Object::toString).collect(Collectors.joining(", ")) | list.mkString(", ")
| Drop from head | ? | list.drop(2) - list without first n elements |
| Drop from tail | ? | list.dropRight(2) - list without it's last n elements |
| Iteration Execution | .forEach(Consumer) | list.foreach(x => println(x)) | 
| Head| ? | list.head |
| Last | ? | list.last |
| Tail| ? | list.tail | 
| Init | ? | list.init |
| Empty? | ? | list.isEmpty | 
| Size | ? | list.length |
| Filter Not | n/a | list.filterNot(...) | 
| Reverse | ? | list.reverse | 
| Sort | ? | list.sort(comparison f) | 
| Distinct | .distinct() | list.distinct() |
| Indexed access | N/A | list(index) |
| Filter Count  | .filter(predicate).count() | list.count() |
| Flat Map | | flatMap() |
| Flatten |  | flatten |
| Group By | | groupBy[K](f: (A) ⇒ K): Map[K, List[A]] |
| Max | | max |
| Max By | | maxBy
| Min  | | min
| Min By | | minBy
| Sum | | sum |
