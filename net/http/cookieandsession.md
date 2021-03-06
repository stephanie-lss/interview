### Cookie & Session 

首先需要值得注意的一个事实是，HTTP协议是无状态的协议，如果出去webserver和浏览器其维护的keep-alive，那么每次请求和响应都是没有关联的。这个时候问题就出现了，如何维护用户的状态呢？最简单的例子是用户在浏览网页的时候把几件商品加入了购物车，但是并没有登录，如何记录这些商品，并在下次用户登录的时候能够将这些商品加入到购物车。

这就需要服务端维护一个用户的状态，该状态可以存在内存中，数据库中，甚至放在磁盘的文件中。等到下次用户再此请求网页的时候，服务器就会根据用户的状态返回不一样的内容。这个很简单的 存在服务端的 状态 就被成为session。

再把问题复杂一点点，两个用户同时请求网页，如何获取当前用户在服务端的session呢？这就需要我们在客户端或是说浏览器也维护一个状态，每次用户请求服务器资源时就会带着这个状态。如果简单来说这个状态是为了唯一表示一个服务端的session的，我们应该称之为session ID，但我们给它一个更加fashion的名字为cookie。

cookie在用户第一次向服务端请求资源时就会生成，然后下发给客户端，以后的每次资源请求都会带着这个东西以告诉服务端自己是那个尊贵的客人。

cookie的存在知识为了区分不同的用户，更多的其实我们可以有更多广泛意义的cookie，比如其实用户名+密码也是某种意义上的cookie，用户设备的唯一标识码也是，只要一个东西能够 唯一 的标识服务端的一个用户，那么这个东西就是广义上的cookie。

或者我们还可以把服务端的session搬到客户端，然后把它加密，这时候我们就称之为加密的cookie，不过该方式始终还要在服务端维护一个信息的，这个时候用户的用户名加上密码就成了真正意义上的cookie了，这个cookie在本地的加密的cookie需要废除的时候，就会用来和服务端进行同步用。