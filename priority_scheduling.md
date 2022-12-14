


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