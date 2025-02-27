[中文版](/docs/docs-02-service.md)

## RPC Service

- It is the basic unit for SRPC services.
- Each service must be generated by one type of IDLs.
- Service is determined by IDL type, not by specific network communication protocol.

### Sample

You can follow the detailed example below:

- Use the same `example.proto` IDL above.
- Run the official `protoc example.proto --cpp_out=./ --proto_path=./` to get two files: `example.pb.h` and `example.pb.cpp`.
- Run the `srpc_generator protobuf ./example.proto ./` in SRPC to get `example.srpc.h`.
- Derive `Example::Service` to implement the rpc business logic, which is an RPC Service.
- Please note that this Service does not involve any concepts such as network, port, communication protocol, etc., and it is only responsible for completing the business logic that convert `EchoRequest` to `EchoResponse`.

~~~cpp
class ExampleServiceImpl : public Example::Service
{
public:
    void Echo(EchoRequest *request, EchoResponse *response, RPCContext *ctx) override
    {
        response->set_message("Hi, " + request->name());

        printf("get_req:\n%s\nset_resp:\n%s\n",
                request->DebugString().c_str(),
                response->DebugString().c_str());
    }
};
~~~

