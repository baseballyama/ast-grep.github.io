# Scan Your Project!

Now you have learnt the basic of ast-grep's pattern syntax and searching.
Let's explore its power to run scan on your code repository in a scalable way!

`sg scan` is the command you can use to run multiple rules against your repository so that you don't need to pass pattern query to your command line every time.

However, to ast-grep's scan need some scaffolding for project setup. We will walk through the process in this guide.

## Create scaffolding...

To set up ast-grep's scanning, you can simply run the command `sg new` in the root directory of your repository. You will be guided with a series of interactive questions, like the following:

```markdown
No sgconfig.yml found. Creating a new ast-grep project...
> Where do you want to have your rules? rules
> Do you want to create rule tests? Yes
> Where do you want to have your tests? rule-test
> Do you want to create folder for utility rules? Yes
> Where do you want to have your utilities? utils
Your new ast-grep project has been created!
```

After you answering these questions, you will get a folder structure like the below.

```bash
my-awesome-project
  |- rules           # where rules go
  |- rule-test       # test cases for rules
  |- utils           # global utility rules for reusing
  |- sgconfig.yml    # root configuration file
```

## Create the rule!

Now you can start creating a rule! Continue using `sg new`, it will ask you what to create. But you can also use `sg new rule` to create a rule directly!

You will be asked several questions about the rule going to be created. Suppose we want to create a rule to disable no-eval in JavaScript.

```markdown
> What is your rule's name? no-eval
> Choose rule's language JavaScript
Created rules at ./rules/no-eval.yml
> Do you also need to create a test for the rule? Yes
Created test at rule-test/no-eval-test.yml
```

Now you can see open the new rule created in the `rules/no-eval.yml`. File path might vary depending on your choice on the first step.

> `no-eval.yml`

```yml
id: no-eval
message: Add your rule message here....
severity: error # error, warning, hint, info
language: JavaScript
rule:
  pattern: Your Rule Pattern here...
# utils: Extract repeated rule as local utility here.
# note: Add detailed explanation for the rule.
```

We will go through the rule config in the next chapter. But these configurations are quite obvious and self explaining.

Let's change the `pattern` inside `rule` and change the rule's message.

```yml
id: no-eval
message: Do not use eval! Dangerous! Hazardous! Perilous!
severity: error
language: JavaScript
rule:
  pattern: eval($CODE)
```

Okay! The pattern syntax works just like what we have learnt before.

## `sg scan` the code

Now you can try scannning the code! You can create a JavaScript file containing `eval` to test it.

Run `sg scan` in your project, ast-grep will give you some beautiful scan report!

```bash
error[no-eval]: Add your rule message here....
  ┌─ ./test.js:1:1
  │
1 │ eval('hello')
  │ ^^^^^^^^^^^^^

Error: 1 error(s) found in code.
Help: Scan succeeded and found error level diagnostics in the codebase.
```


## Summary

In this section we learnt how to set up ast-grep project, create new rules using cli tool and scan problems in the repository.

To summarize the commands we used:

* `sg new` - Create a new ast-grep project
* `sg new rule` - Create a new rule in a rule folder.
* `sg scan` - Scan the codebase with the rules in the project.