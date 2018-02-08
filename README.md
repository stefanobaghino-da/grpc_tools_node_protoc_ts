grpc_tools_node_protoc_ts
=========================

## Aim
Generate corresponding d.ts codes according to js codes generated by [grpc_tools_node_protoc](https://www.npmjs.com/package/grpc-tools)

More information about grpc_tools_node_protoc:

* [npm](https://www.npmjs.com/package/grpc-tools)
* [source code](https://github.com/grpc/grpc-node/tree/master/packages/grpc-tools)
* [doc how to use](https://github.com/grpc/grpc/blob/master/examples/node/static_codegen/README.md)

## Breaking changes
Since v2.x.x, current project supports the official definition of grpc, see: [index.d.ts](https://github.com/grpc/grpc-node/blob/v1.8.x/packages/grpc-native-core/index.d.ts). 
Though the usage of tool, and generated codes shall not been changed, it's good to be double checked in your project when upgrade.

TSLint has been disabled in generated files. Please see the conversation: [#13](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/13).

~~## Note~~
~~This tools is using an unofficial grpc.d.ts definition, see: [grpc-tsd](https://www.npmjs.com/package/grpc-tsd).~~
~~If you want to use this tool, you have to use definition mentioned.~~

## How to use
```bash
npm install grpc_tools_node_protoc_ts --save-dev

# generate js codes via grpc-tools
grpc_tools_node_protoc \
--js_out=import_style=commonjs,binary:./your_dest_dir \
--grpc_out=./your_dest_dir \
--plugin=protoc-gen-grpc=`which grpc_tools_node_protoc_plugin` \
-I ./proto \
./your_proto_dir/*.proto

# generate d.ts codes
protoc \
--plugin=protoc-gen-ts=./node_modules/.bin/protoc-gen-ts \
--ts_out=./your_dest_dir \
-I ./proto \
./your_proto_dir/*.proto
```

## Sample
There is a complete & runnable sample in folder `examples`.

Dirs:

* proto: sample proto definition
* bash: useful commands
    * build.sh: build js & d.ts codes from proto file, and tsc to build/*.js
    * server.sh: start the sample server
    * client.sh: start the client & send requests

### Note
In current grpc version:

> $ npm view grpc time
  ...
  '1.8.4': '2018-01-18T15:17:25.731Z',
  '1.9.0-pre3': '2018-02-02T17:38:06.018Z' }

There is some definition confliction, and affects examples codes.

This possible be the issue of version confliction of protobuf.js in grpc and the newer version used outside.
See: [grpc package.json](https://github.com/grpc/grpc-node/blob/v1.8.x/packages/grpc-native-core/package.json#L35) and [grpc_tools_node_protoc_ts package.json](https://github.com/agreatfool/grpc_tools_node_protoc_ts/blob/v2.1.0/package.json#L40)

The result is, when you tsc under examples, there is some error:

> $ cd ./examples
  $ ./bash/build.sh
  src/server.ts(15,21): error TS2345: Argument of type 'IBookServiceService' is not assignable to parameter of type 'Service'.
    Property 'methods' is missing in type 'IBookServiceService'.

Please ignore it, if dir examples is copied to any other place from current project root, it works fine. Anyway even with these errors, js codes from tsc also works fine.

### book.proto
```proto
syntax = "proto3";

package com.book;

message Book {
    int64 isbn = 1;
    string title = 2;
    string author = 3;
}

message GetBookRequest {
    int64 isbn = 1;
}

message GetBookViaAuthor {
    string author = 1;
}

service BookService {
    rpc GetBook (GetBookRequest) returns (Book) {}
    rpc GetBooksViaAuthor (GetBookViaAuthor) returns (stream Book) {}
    rpc GetGreatestBook (stream GetBookRequest) returns (Book) {}
    rpc GetBooks (stream GetBookRequest) returns (stream Book) {}
}

message BookStore {
    string name = 1;
    map<int64, string> books = 2;
}

enum EnumSample {
    option allow_alias = true;
    UNKNOWN = 0;
    STARTED = 1;
    RUNNING = 1;
}
```

### book_pb.d.ts
```typescript
// package: com.book
// file: book.proto

/* tslint:disable */

import * as jspb from "google-protobuf";

export class Book extends jspb.Message { 
    getIsbn(): number;
    setIsbn(value: number): void;

    getTitle(): string;
    setTitle(value: string): void;

    getAuthor(): string;
    setAuthor(value: string): void;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): Book.AsObject;
    static toObject(includeInstance: boolean, msg: Book): Book.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: Book, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): Book;
    static deserializeBinaryFromReader(message: Book, reader: jspb.BinaryReader): Book;
}

export namespace Book {
    export type AsObject = {
        isbn: number,
        title: string,
        author: string,
    }
}

export class GetBookRequest extends jspb.Message { 
    getIsbn(): number;
    setIsbn(value: number): void;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): GetBookRequest.AsObject;
    static toObject(includeInstance: boolean, msg: GetBookRequest): GetBookRequest.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: GetBookRequest, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): GetBookRequest;
    static deserializeBinaryFromReader(message: GetBookRequest, reader: jspb.BinaryReader): GetBookRequest;
}

export namespace GetBookRequest {
    export type AsObject = {
        isbn: number,
    }
}

export class GetBookViaAuthor extends jspb.Message { 
    getAuthor(): string;
    setAuthor(value: string): void;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): GetBookViaAuthor.AsObject;
    static toObject(includeInstance: boolean, msg: GetBookViaAuthor): GetBookViaAuthor.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: GetBookViaAuthor, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): GetBookViaAuthor;
    static deserializeBinaryFromReader(message: GetBookViaAuthor, reader: jspb.BinaryReader): GetBookViaAuthor;
}

export namespace GetBookViaAuthor {
    export type AsObject = {
        author: string,
    }
}

export class BookStore extends jspb.Message { 
    getName(): string;
    setName(value: string): void;


    getBooksMap(): jspb.Map<number, string>;
    clearBooksMap(): void;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): BookStore.AsObject;
    static toObject(includeInstance: boolean, msg: BookStore): BookStore.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: BookStore, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): BookStore;
    static deserializeBinaryFromReader(message: BookStore, reader: jspb.BinaryReader): BookStore;
}

export namespace BookStore {
    export type AsObject = {
        name: string,

        booksMap: Array<[number, string]>,
    }
}

export enum EnumSample {
    UNKNOWN = 0,
    STARTED = 1,
    RUNNING = 1,
}
```

### book_grpc_pb.d.ts
```typescript
// package: com.book
// file: book.proto

/* tslint:disable */

import * as grpc from "grpc";
import * as book_pb from "./book_pb";

interface IBookServiceService extends grpc.ServiceDefinition {
    getBook: IGetBook;
    getBooksViaAuthor: IGetBooksViaAuthor;
    getGreatestBook: IGetGreatestBook;
    getBooks: IGetBooks;
}

interface IGetBook {
    path: string; // "/com.book.BookService/GetBook"
    requestStream: boolean; // false
    responseStream: boolean; // false
    requestType: book_pb.GetBookRequest;
    responseType: book_pb.Book;
    requestSerialize: (arg: book_pb.GetBookRequest) => Buffer;
    requestDeserialize: (buffer: Uint8Array) => book_pb.GetBookRequest;
    responseSerialize: (arg: book_pb.Book) => Buffer;
    responseDeserialize: (buffer: Uint8Array) => book_pb.Book;
}
interface IGetBooksViaAuthor {
    path: string; // "/com.book.BookService/GetBooksViaAuthor"
    requestStream: boolean; // false
    responseStream: boolean; // true
    requestType: book_pb.GetBookViaAuthor;
    responseType: book_pb.Book;
    requestSerialize: (arg: book_pb.GetBookViaAuthor) => Buffer;
    requestDeserialize: (buffer: Uint8Array) => book_pb.GetBookViaAuthor;
    responseSerialize: (arg: book_pb.Book) => Buffer;
    responseDeserialize: (buffer: Uint8Array) => book_pb.Book;
}
interface IGetGreatestBook {
    path: string; // "/com.book.BookService/GetGreatestBook"
    requestStream: boolean; // true
    responseStream: boolean; // false
    requestType: book_pb.GetBookRequest;
    responseType: book_pb.Book;
    requestSerialize: (arg: book_pb.GetBookRequest) => Buffer;
    requestDeserialize: (buffer: Uint8Array) => book_pb.GetBookRequest;
    responseSerialize: (arg: book_pb.Book) => Buffer;
    responseDeserialize: (buffer: Uint8Array) => book_pb.Book;
}
interface IGetBooks {
    path: string; // "/com.book.BookService/GetBooks"
    requestStream: boolean; // true
    responseStream: boolean; // true
    requestType: book_pb.GetBookRequest;
    responseType: book_pb.Book;
    requestSerialize: (arg: book_pb.GetBookRequest) => Buffer;
    requestDeserialize: (buffer: Uint8Array) => book_pb.GetBookRequest;
    responseSerialize: (arg: book_pb.Book) => Buffer;
    responseDeserialize: (buffer: Uint8Array) => book_pb.Book;
}

export interface IBookServiceClient {
    getBook(request: book_pb.GetBookRequest, callback: (error: Error | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    getBook(request: book_pb.GetBookRequest, metadata: grpc.Metadata, callback: (error: Error | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    getBooksViaAuthor(request: book_pb.GetBookViaAuthor, metadata?: grpc.Metadata): grpc.ClientReadableStream;
    getGreatestBook(callback: (error: Error | null, response: book_pb.Book) => void): grpc.ClientWritableStream;
    getGreatestBook(callback: (error: Error | null, metadata: grpc.Metadata, response: book_pb.Book) => void): grpc.ClientWritableStream;
    getBooks(): grpc.ClientDuplexStream;
    getBooks(metadata: grpc.Metadata): grpc.ClientDuplexStream;
}

export const BookServiceService: IBookServiceService;
export class BookServiceClient extends grpc.Client implements IBookServiceClient {
    constructor(address: string, credentials: grpc.ChannelCredentials, options?: object);
    public getBook(request: book_pb.GetBookRequest, callback: (error: Error | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    public getBook(request: book_pb.GetBookRequest, metadata: grpc.Metadata, callback: (error: Error | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    public getBooksViaAuthor(request: book_pb.GetBookViaAuthor, metadata?: grpc.Metadata): grpc.ClientReadableStream;
    public getGreatestBook(callback: (error: Error | null, response: book_pb.Book) => void): grpc.ClientWritableStream;
    public getGreatestBook(callback: (error: Error | null, metadata: grpc.Metadata, response: book_pb.Book) => void): grpc.ClientWritableStream;
    public getBooks(metadata?: grpc.Metadata): grpc.ClientDuplexStream;
}
```

## Environment
```bash
node --version
# v8.4.0
npm --version
# 5.2.0
protoc --version
# libprotoc 3.3.0
grpc_tools_node_protoc --version
# libprotoc 3.4.0
npm list -g --depth=0 | grep grpc-tools
# grpc-tools@1.6.6
```