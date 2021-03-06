---
title: Introduction
description: Powerful data validation
layout: gem-single
type: gem
name: dry-validation
---

> This documentation is a WIP while we're in 1.0.0 alpha/beta phase

`dry-validation` is a data validation library that provides a powerful DSL for defining schemas and validation rules.

## Quick start

Validations are expressed through contract objects. A contract specifies a schema with basic type checks and any additional rules that should be applied. Contract rules are applied only when values that they rely on have been succesfully verified by the schema.

Here's an example contract:

``` ruby
class NewUserContract < Dry::Validation::Contract
  params do
    required(:email).filled(:string)
    required(:age).value(:integer)
  end

  rule(:email) do
    key.failure('is already taken') if User.where(email: values[:email]).count > 0
  end

  rule(:age) do
    key.failure('must be greater than 18') if values[:age] < 18
  end
end

contract = NewUserContract.new

contract.call(email: 'jane@doe.org', age: '17')
# #<Dry::Validation::Result{:email=>"jane@doe.org", :age=>17} errors={:age=>["must be greater than 18"]}>
```