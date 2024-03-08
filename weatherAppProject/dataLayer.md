data layer contains application data and business logic.

business logic is what gives value to you app.

it determines how application data must be created, stored and changed.

data layer made of repositores can intercat with 0,1 or more dataSources.

dataSource= are responsible for providing data the app needs to function.
they may present data from the network , a local database, files or even
from memory and they should
have the responsibility of working only with one source of data. which
typically holds a unit of
business logic.

repository layer responsible for exposing data to the rest of the app but
also , for centralizing
changes to the data.
resolving conflicts between multiple datasources,and containg business
logic.
you should create repository for each diffrent type of data you handle in
your app.

your repository can take multiple datasource .

all layers in app should never depend on datasource directly. and they
always use repository.

a common pattern in repositories is to perform one-shot calls. like
create , read, update ad delete.
you can implement this by suspend function with coroutine and also you can
be notified of data
changes over time by exposing data stream with kotlin flow.
when you return data from data base your single source of truth is
database.

the data expose by the data layer should be immutable so that all the
classes cannot tamper with it.

advantage of immutability data is that it can be safely handled by
multiple threads. kotlin data
classes are perfect tool for this.
and the entity and data class in data layer might not be what the other
layers need.

calling data source and repository should be main-safe. safe to call from
the main thread. and
whould use withContext(IO) and our data should use the background thread
for use network operation.

and for error handling we can use catch from flow .

our repository can have multiple repository ??? why?
 











 




