# Lecture1
Thread State
* Program counter and
* Local variables

System state
* Object fields (shared variables) and
* Union of thread states

Events of two or more threads
* Interleaved
* Not necessarily independent (why?)

严格的偏序
* Irreflexive
* Antisymmetric
* Transitive

for every distinct A, B,

Either A → B or B → A

一定要有共享变量来传递共享信息。