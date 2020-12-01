# Clean Code

These are Andculture's code standards based on Robert Martin's _Clean Code_.

## Meaningful Names

### Make Meaningful Distinctions

#### Avoid Noise Words

Noise words are words that don't provide additional information, aren't known patterns used in
software, and are often added to differentiate similar names. Examples of noise words:

1.  Amount
1.  Data
1.  Info
1.  Object
1.  The
1.  (Any name that includes the item type)

Instead of using words like these, add context to the names. A single name may not be incorrect, but
when existing alongside similar names, it becomes a problem.

| Bad                                            | Good                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| Product<br>ProductData<br>ProductInfo          | ProductDetail<br>ProductListItem<br>HomePageProduct          |
| GetActiveAccount()<br>GetActiveAccountRecord() | GetActiveAccountFullDetails()<br>GetActiveAccountForHeader() |

#### Names Should have Meaningful Distinctions

Being able to differentiate classes, methods or variables isn't enough, they should have meaningful
distinctions. This means that we shouldn't just iterate a number over items. The names should show
why they are different than the other similar objects. In tests the distinction may be whether the
result is expected or not. Keep in mind that if you are creating multiple records that do not need
to be read, they could be unnamed and initiated within an array.

| Bad                                                                                                                                                | Good                                                                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| let user1 = null;<br>let user2 = null;<br>let user3 = null;                                                                                        | let studentUser = null;<br>let parentUser = null;<br>let teacherUser = null;                                                                                                  |
| let product1 = null;<br>let product2 = null;                                                                                                       | let expectedProduct = null;<br>let unexpectedProduct = null;                                                                                                                  |
| let expectedWidget = create(1);<br>let unexpectedWidget2 = create(2);<br>let unexpectedWidget3 = create(3);<br> let unexpectedWidget4 = create(4); | let expectedWidget = create(1);<br>const options = [ expectedWidget, create(2), create(3), create(4) ];<br>/\* assert on results length rather than not having unexpected \*/ |
