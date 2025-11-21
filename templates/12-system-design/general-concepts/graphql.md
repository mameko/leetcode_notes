# GraphQL

感觉跟ElasticSearch差不多，都是一种DB然后提供了一种api去query。这个GraphQL其实是个spec，只要按照这个spec做的东西都有这些好处。

譬如，可以解决RESTful api，over/under-fetching data问题，因为graphQL里client想要什么server就send back什么，不像rest那样，都订好了，不能随时按需要改变。

这里还提到可以solve n+1 problem，就是，一个api可能需要很多个sub api calls来组成。如果能customize，那么就能省掉其他的call了，rest不行。（之前看到netflex eng blog里也有同样的问题，但他们用grpc，所以利用field mask来解决

[https://netflixtechblog.com/practical-api-design-at-netflix-part-1-using-protobuf-fieldmask-35cfdc606518](https://netflixtechblog.com/practical-api-design-at-netflix-part-1-using-protobuf-fieldmask-35cfdc606518)

[https://netflixtechblog.com/practical-api-design-at-netflix-part-2-protobuf-fieldmask-for-mutation-operations-2e75e1d230e4](https://netflixtechblog.com/practical-api-design-at-netflix-part-2-protobuf-fieldmask-for-mutation-operations-2e75e1d230e4)）

[https://progressivecoder.com/the-one-introduction-to-graphql-you-definitely-need/](https://progressivecoder.com/the-one-introduction-to-graphql-you-definitely-need/)

[https://dzone.com/articles/graphql-core-concepts-you-should-definitely-know](https://dzone.com/articles/graphql-core-concepts-you-should-definitely-know)
