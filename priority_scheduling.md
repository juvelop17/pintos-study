

<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> fbb8fcc (priority-change priority-fifo priority-preempt)
>>>>>>> 06c6f2b (priority-change priority-fifo priority-preempt)
```
bool cmp_priority(struct list_elem *a, struct list_elem *b, void *aux) {
	return list_entry(a, struct thread, elem)->priority > list_entry(b, struct thread, elem)->priority;
}
```

<<<<<<< HEAD


=======
<<<<<<< HEAD


=======
>>>>>>> fbb8fcc (priority-change priority-fifo priority-preempt)
>>>>>>> 06c6f2b (priority-change priority-fifo priority-preempt)
```
struct list_elem *
list_front(struct list *list)
{
	ASSERT(!list_empty(list));
	return list->head.next;
}
<<<<<<< HEAD
```
=======
<<<<<<< HEAD
```
=======
```


pass tests/threads/priority-change
FAIL tests/threads/priority-donate-one
FAIL tests/threads/priority-donate-multiple
FAIL tests/threads/priority-donate-multiple2
FAIL tests/threads/priority-donate-nest
FAIL tests/threads/priority-donate-sema
FAIL tests/threads/priority-donate-lower
pass tests/threads/priority-fifo
pass tests/threads/priority-preempt
FAIL tests/threads/priority-sema
FAIL tests/threads/priority-condvar
FAIL tests/threads/priority-donate-chain
>>>>>>> fbb8fcc (priority-change priority-fifo priority-preempt)
>>>>>>> 06c6f2b (priority-change priority-fifo priority-preempt)
