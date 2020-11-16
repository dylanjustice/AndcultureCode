# Clean Code by Robert C. Martin

## Chapter 2: Meaningful Names

### Make Meaningful Distinctions

#### Avoid Noise Words

Noise words are words that don't provide additional information, and are often added to
differentiate similar names.

Examples of noise words:

1.  Amount
1.  Data
1.  Info
1.  Object
1.  Record
1.  The
1.  (Any name that includes the item type)

Instead of using these words, add context to the names. A single name may not be incorrect, but
when existing alongside similar names, it becomes a problem.

| Bad:                                           | Good:                                                        |
| ---------------------------------------------- | ------------------------------------------------------------ |
| Product<br>ProductData<br>ProductInfo          | ProductDetail<br>ProductListItem<br>HomePageProduct          |
| GetActiveAccount()<br>GetActiveAccountRecord() | GetActiveAccountForHeader()<br>GetActiveAccountFullDetails() |

#### Names Should have Meaningful Distinctions

Being able to differentiate classes, methods or variables isn't enough, they should have meaningful
distinctions. This means that we shouldn't just iterate a number over items. The names should show
why they are different than the other similar objects.

Bad:

| Bad                                                         | Good                                                                         |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------- |
| let user1 = null;<br>let user2 = null;<br>let user3 = null; | let studentUser = null;<br>let parentUser = null;<br>let teacherUser = null; |

#### Allowed Names

There are some naming conventions that we will allow. We decided that we do not have problems with
