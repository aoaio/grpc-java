grpc-java
=========

develop grpc client by using java


**编译.proto文件**

我们需要从 .proto 的服务定义中生成 gRPC 客户端和服务器端的接口。我们通过 protocol buffer 的编译器 protoc 以及一个特殊的 gRPC Java 插件来完成。编译器protoc 与 gRPC Java 插件下载的地址己经写在plugins.txt文件中。


* ubuntu系统

安装protoc编译器

sudo apt-get install libprotoc-dev

执行 *.sh文件
----
xx@xx-ubuntu:~/workspace/intellij/grpc-java/src/main/proto$ ./account/account.sh

成功后生成AccountGrpc.java 与 AccountProto.java 


**编写Java gRPC client：AccountClient.java**

```Java
private final ManagedChannel channel;
private AccountGrpc.AccountBlockingStub blockingStub;

public AccountClient(String host, int port) {
    channel = ManagedChannelBuilder.forAddress(host, port)
            .usePlaintext()
            .build();

    blockingStub = AccountGrpc.newBlockingStub(channel);
}
    
public List<ByteString> accounts() {
    CommonProto.AccountsRequest request = CommonProto.AccountsRequest.newBuilder().build();
    CommonProto.AccountsReply reply = blockingStub.accounts(request);
    if (reply == null) {
        return null;
    }

    List<ByteString> accounts = reply.getAccountsList();
    for (ByteString account : accounts) {
        System.out.println(Utils.byteStringToHex(account));
    }
    return accounts;
}
```
