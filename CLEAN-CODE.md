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

### Don't Add Gratuitous Context

When a variable name can be inferred from the context of the class, interface or other actor you're in, prefixing the variable with the additional context adds no value. Extra context should only be added for actors that differ from the domain/context.

For example, in an API controller that performs read operations on a `Chapter` entity, your first instinct might be to name the repository conductor prefixed with "chapter":

```CS
[FormatFilter]
[ApiRoute("chapters")]
public class ChaptersController : ApiController
{
    #region Private Properties

    // The "chapter" prefix here is redundant due to the context of being in the `ChaptersController`
    private readonly IRepositoryReadConductor<Chapter> _chapterReadConductor;
    private readonly ILogger<ChaptersController> _logger;
    private readonly IMapper _mapper;

    #endregion Private Properties

    #region Constructor

    public ChaptersController(
        IRepositoryReadConductor<Chapter> chapterReadConductor,
        IStringLocalizer localizer,
        ILogger<ChaptersController> logger,
        IMapper mapper
    ) : base(localizer)
    {
        _chapterReadConductor = chapterReadConductor;
        _logger = logger;
        _mapper = mapper;
    }

    #endregion Constructor

    // Get, Index, etc. controller methods here
}
```

However, in this context, we know that the primary entity we are working with is a `Chapter`. Prefixing the conductor with "chapter" is superfluous and can be removed. This would be more concise and provide the same amount of information:

```CS
[FormatFilter]
[ApiRoute("chapters")]
public class ChaptersController : ApiController
{
    #region Private Properties

    private readonly ILogger<ChaptersController> _logger;
    private readonly IMapper _mapper;
    // Since the entity for this conductor is our primary entity in this controller, we don't need to prefix it with "chapter"
    private readonly IRepositoryReadConductor<Chapter> _readConductor;

    #endregion Private Properties

    #region Constructor

    public ChaptersController(
        IStringLocalizer localizer,
        ILogger<ChaptersController> logger,
        IMapper mapper,
        IRepositoryReadConductor<Chapter> readConductor,
    ) : base(localizer)
    {
        _logger = logger;
        _mapper = mapper;
        _readConductor = readConductor;
    }

    #endregion Constructor

    // Get, Index, etc. controller methods here
}
```

### Add Meaningful Context

Of course, when there are additional actors that do not directly relate to the domain entity, it then becomes appropriate to make the distinction:

```CS
[FormatFilter]
[ApiRoute("chapters")]
public class ChaptersController : ApiController
{
    #region Private Properties

    private readonly ILogger<ChaptersController> _logger;
    private readonly IMapper _mapper;
    // Since this entity now differs from our primary entity `Chapter`, we will make the distinction in the name.
    private readonly IRepositoryReadConductor<Publication> _publicationReadConductor;
    private readonly IRepositoryReadConductor<Chapter> _readConductor;

    #endregion Private Properties

    #region Constructor

    public ChaptersController(
        IStringLocalizer localizer,
        ILogger<ChaptersController> logger,
        IMapper mapper,
        IRepositoryReadConductor<Publication> publicationReadConductor,
        IRepositoryReadConductor<Chapter> readConductor,
    ) : base(localizer)
    {
        _logger = logger;
        _mapper = mapper;
        _publicationReadConductor = publicationReadConductor;
        _readConductor = readConductor;
    }

    #endregion Constructor

    // Get, Index, etc. controller methods here
}
```
