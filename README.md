# AWS Lambda Rust Runtime Sample

Based on https://github.com/awslabs/aws-lambda-rust-runtime.

## Prerequisites
- If running on Mac OS, install the linker to an `x86_64-unknown-linux-musl` platform.
  - see https://github.com/awslabs/aws-lambda-rust-runtime#aws-cli
- Create an IAM user to develop Lambda
  - attach `AWSLambda_FullAccess`
  - see https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html
- Create a Lambda execution role
  - attach `AWSLambdaBasicExecutionRole`
  - see https://docs.aws.amazon.com/lambda/latest/dg/runtimes-walkthrough.html


## Deployment via AWS CLI

```sh
$ cargo build --release --target x86_64-unknown-linux-musl
```

```sh
$ cp ./target/x86_64-unknown-linux-musl/release/aws_lambda_sample ./bootstrap && zip lambda.zip bootstrap && rm bootstrap
```

```sh
aws lambda create-function --function-name rustTest \
--handler doesnt.matter \
--zip-file fileb://./lambda.zip \
--runtime provided \
--role {ARN OF EXECUTION ROLE} \
--environment Variables={RUST_BACKTRACE=1} \
--tracing-config Mode=Active \
--profile {YOUR AWS PROFILE}
```
