# EbenezerCommunityLibrary
A java program for Ebenezer Community Library

NOTES, DESIGN CHOICES & SHORT WRITE-UP
Data structure choices (why)
•	TreeMap<String, List<Book>> booksByCategory — stores books grouped by category and keeps categories sorted automatically. Good for reporting and category filtering.
•	HashMap<String, Book> booksByIsbn — O(1) lookup for ISBN operations (borrow/return).
•	TreeMap + HashMap for borrowers — TreeMap provides a tree structure (sorted by ID) so we can demonstrate recursive traversals; HashMap provides fast direct lookup by ID for runtime operations.
•	ArrayList for transaction history — dynamic array to keep transaction order, easy to iterate and scan for reports.
•	Queue (LinkedList) for lending queue — models incoming borrow requests processed FIFO.
•	Stack for returns demo — included to demonstrate LIFO mechanics if you want to use it for certain workflows.
•	PriorityQueue for overdueTracker — min-heap ordered by dueDate so earliest due items are processed first (fast for identifying overdue items).
Algorithms implemented (search & sort) and tradeoffs
•	Linear search (title/author): O(n) — easy to implement, fine for small/medium collections or substring searches. Used for flexible queries.
•	Binary search (title): O(log n) — faster on large collections but requires sorted list and returns exact-match; less useful for substring searches.
•	Merge sort for book title sorting: O(n log n) average/worst/best (Ω: n log n). Stable and efficient for large lists — chosen as the primary sort.
•	Selection sort demo for tiny lists: O(n²) — simple but inefficient; included for demonstration/teaching when n is small or for in-place small sorts.
Pros/Cons in this use case
•	Merge sort: great for sorting catalog titles ahead of binary search; memory overhead modest due to temporary lists.
•	Binary search: very fast for exact title lookups, but requires exact normalized title and sorted order.
•	Linear search: flexible (substrings), but slower on large catalogs. Acceptable given offline modest-scale library.
•	HashMap lookups for ISBN/borrower are essentially O(1) average — fast and appropriate for frequent operations like lending and returns.
Recursion
•	Implemented a recursive function recursiveFindBorrower to demonstrate recursive traversal over borrower keys (educational — in practice you'd use direct map lookup).
•	Merge sort itself uses recursion to split and merge lists — standard divide-and-conquer.
Overdue & fines policy
•	The system tracks dueDate for each transaction.
•	Overdue grace: 14 days. If a book is more than 14 days overdue, the borrower is fined 1.0 currency unit per day after the grace period. (You can change OVERDUE_FREE_DAYS and FINE_PER_DAY constants near the top of the file.)
File storage format
•	Simple CSV lines (fields separated by commas). To avoid breaking CSV, commas are replaced with semicolons in user strings (naive but practical for console usage). Files are books.txt, borrowers.txt, and transactions.txt. On start the app reloads these files.
Reports
•	Most borrowed books in past 30 days: scans transactions and counts borrows per ISBN.
•	Borrowers with highest outstanding fines: sorts borrower list by fines.
•	Inventory distribution by category: counts titles and copies from booksByCategory.
Performance analysis (short)
•	Loading & saving: file I/O is O(n) in number of items.
•	Borrow/return: O(1) hashmap operations for book & borrower lookup, O(log n) for heap operations when managing overdue queue.
•	Reporting (most-borrowed): O(T) to scan transaction list where T is number of transactions.

