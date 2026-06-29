# Protobuf Service Starter

```proto
syntax = "proto3";
package user.v1;

service UserService {
  rpc GetUser(GetUserRequest) returns (User);
  rpc ListUsers(ListUsersRequest) returns (stream User);
}

message GetUserRequest { string id = 1; }
message ListUsersRequest { int32 limit = 1; string cursor = 2; }
message User { string id = 1; string email = 2; }
```

Rules: add fields never reuse tags; reserve removed; version in package (`v1`); status via gRPC codes.
