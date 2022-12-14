
```
bool cmp_priority(struct list_elem *a, struct list_elem *b, void *aux) {
	return list_entry(a, struct thread, elem)->priority > list_entry(b, struct thread, elem)->priority;
}
```

```
struct list_elem *
list_front(struct list *list)
{
	ASSERT(!list_empty(list));
	return list->head.next;
}
```

```
bool cmp_priority(struct list_elem *a, struct list_elem *b, void *aux)
{
	return list_entry(a, struct thread, elem)->priority > list_entry(b, struct thread, elem)->priority;
}
```

```
void thread_current_priority_check()
{
	if (!list_empty(&ready_list) && thread_current()->priority < list_entry(list_front(&ready_list), struct thread, elem)->priority)
	{
		thread_yield();
	}
}
```

```
tid_t thread_create(const char *name, int priority,
					thread_func *function, void *aux)
{
	struct thread *t;
	tid_t tid;
	ASSERT(function != NULL);
	/* Allocate thread. */
	t = palloc_get_page(PAL_ZERO);
	if (t == NULL)
		return TID_ERROR;
	/* Initialize thread. */
	init_thread(t, name, priority);
	tid = t->tid = allocate_tid();
	/* Call the kernel_thread if it scheduled.
	 * Note) rdi is 1st argument, and rsi is 2nd argument. */
	t->tf.rip = (uintptr_t)kernel_thread;
	t->tf.R.rdi = (uint64_t)function;
	t->tf.R.rsi = (uint64_t)aux;
	t->tf.ds = SEL_KDSEG;
	t->tf.es = SEL_KDSEG;
	t->tf.ss = SEL_KDSEG;
	t->tf.cs = SEL_KCSEG;
	t->tf.eflags = FLAG_IF;
	/* Add to run queue. */
	thread_unblock(t);

	/* compare the priorities of the currently running thread and the newly inserted one.
	Yield the CPU if the newly arriving thread has higher priority*/
	thread_current_priority_check();

	return tid;
}


```


```
void thread_unblock(struct thread *t)
{
	enum intr_level old_level;
	ASSERT(is_thread(t));

	old_level = intr_disable();
	ASSERT(t->status == THREAD_BLOCKED);

	// list_push_back(&ready_list, &t->elem);
	list_insert_ordered(&ready_list, &t->elem, cmp_priority, NULL);

	t->status = THREAD_READY;
	intr_set_level(old_level);
}


```


```
void thread_yield(void)
{
	struct thread *curr = thread_current();
	enum intr_level old_level;
	ASSERT(!intr_context());

	old_level = intr_disable();
	if (curr != idle_thread)
		// list_push_back(&ready_list, &curr->elem);
		list_insert_ordered(&ready_list, &curr->elem, cmp_priority, NULL);

	do_schedule(THREAD_READY);
	intr_set_level(old_level);
}

```


```
void thread_set_priority(int new_priority)
{
	thread_current()->priority = new_priority;
	thread_current_priority_check();
}


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
pass tests/threads/priority-sema
pass tests/threads/priority-condvar
FAIL tests/threads/priority-donate-chain
