# jq

## Extract data

Extract the field from a json file

```sh
$ jq '.name' example.json
"Alice"
$ jq '.address.city' example.json
"Wonderland"
```

Extract from an array

```sh
$ jq '.skills[0]' example.json
"Python"
```

## Filter data

```sh
$ jq '{name, age}' example.json 
```


## Arrays

```sh
$ jq '.' example2.json  
[
  {
    "name": "Alice",
    "age": 30
  },
  {
    "name": "Bob",
    "age": 25
  },
  {
    "name": "Charlie",
    "age": 35
  }
]

$ jq '.[]' example2.json
{
  "name": "Alice",
  "age": 30
}
{
  "name": "Bob",
  "age": 25
}
{
  "name": "Charlie",
  "age": 35
}

$ jq '.[] | .name' example2.json
"Alice"
"Bob"
"Charlie"
```

## Selection

```sh
$ jq '.[] | select(.age > 30)' example2.json
{
  "name": "Charlie",
  "age": 35
}

$ jq '.[] | select(.age > 30) | .name' example2.json
"Charlie"
```

## functions

### incrementation

```sh
$ jq '.[] | .age += 1 | .age' example2.json
31
26
36
```

```sh
$ jq '.[] | "age: \(.age)"' example2.json
"age: 30"
"age: 25"
"age: 35"
```

# Use a variable
```sh
$ jq '.[] | .id' example3.json
1
2
3
$ jq --arg city "Wonderland" '.[] | select(.address.city == $city) | .id' example3.json
1
town="Wonderland"
$ jq --arg city $town '.[] | select(.address.city == $city) | .id' example3.json
1
```

