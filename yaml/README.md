# 📄 YAML

Stands for “**YAML ain’t markup language**”. It has 3 key points:

* It’s case sensitive.
* It’s a data serialisation language which means it stores data in form of code. Later using these files we deserialise. It’s similar to JSON and XML.
* It only stores data no commands. So basically it’s **a data format to exchange data**.

It’s used for writing configuration files for Docker and Kubernetes Services.

Benefits:

* Human readable
* Strict syntax and indentation. Using tools like “ValidKube” you can validate YAML files easily.
* Easily convertible to JSON or XML
* Preferred for representing complex data
* Parsing is easy

Sample YAML file:

```yaml
# key-value pairs
”key”:“value”

- - - #separator to indicate different objects

# list
- apple
- mango
- lime

— - -

# block
cities:
 - delhi
 - mumbai

- - -

random: [“hello”, 100]

. . . #end of YAML file
```
