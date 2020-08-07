[![Build Status](https://travis-ci.org/aws-beam/aws-codegen.svg?branch=master)](https://travis-ci.org/aws-beam/aws-codegen)

Code generator for Elixir and Erlang AWS client.

## Generating code

The code generator uses specs from the [AWS SDK for the Go programming
language][aws-sdk-go] to generate code.

Code is generated by running the `generate.exs` script.

**Elixir**

```bash
export SPEC_PATH=../../aws/aws-sdk-go/models/apis
export TEMPLATE_PATH=priv
export ELIXIR_OUTPUT_PATH=../aws-elixir/lib/aws
mix run generate.exs elixir $SPEC_PATH $TEMPLATE_PATH $ELIXIR_OUTPUT_PATH
```

**Erlang**

```bash
export SPEC_PATH=../../aws/aws-sdk-go/models/apis
export TEMPLATE_PATH=priv
export ERLANG_OUTPUT_PATH=../aws-erlang/src
mix run generate.exs erlang $SPEC_PATH $TEMPLATE_PATH $ERLANG_OUTPUT_PATH
```

## AWS Protocols

Each AWS API uses a specific protocol which is defined in its
specification JSON file in the [aws-sdk-go][] repository. The existing
protocols are:

- json
- rest-json
- query
- rest-xml

Every one of these protocols uses HTTP in an asynchronous (request &
response) fashion. They mostly differ in what `Content-Type` they use
for the request body, whether they include parameters in the URL or in
the headers, and what `Content-Type` should one expect in the
response body.

The following table attempts to capture the specifics of each protocol:

|                         | json               | rest-json                               | query                               | rest-xml                                |
|-------------------------|--------------------|-----------------------------------------|-------------------------------------|-----------------------------------------|
| Methods                 | `POST`             | `GET`, `POST`, `PATCH`, `PUT`, `DELETE` | `POST`                              | `GET`, `POST`, `PATCH`, `PUT`, `DELETE` |
| Request `Content-Type`  | `application/json` | `application/json`                      | `application/x-www-form-urlencoded` | `text/xml`                              |
| URL Parameters          | No                 | Yes                                     | No                                  | Yes                                     |
| Header Parameters       | No                 | Yes                                     | No                                  | Yes                                     |
| Response `Content-Type` | `application/json` | `application/json`                      | `text/xml`                          | `text/xml`                              |

[aws-sdk-go]: https://github.com/aws/aws-sdk-go
