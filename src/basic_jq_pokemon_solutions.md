# Practise the Basics of JQ Using Pokemon Data

## Useful Resources
[1] https://earthly.dev/blog/jq-select/
## Solutions
### 1. Write the expression to select the 100th Pokemon
```jq
jq '.[99]' pokemon.json
```

### 2. Select only the Name, Type 1 and Total from the 255th pokemon
```jq
jq '.[254]["Name", "Type 1", "Total"]' pokemon.json
```

### 3. Select Name, Type 1, and Total as an object
```jq
jq '.[254]|{"Name", "Type 1", "Total"}' pokemon.json
```

### 4. Write an expression to select the last 3 Pokémon
```jq
jq '.[-3:]' pokemon.json
```

### 5. Write an expression to find the total number of Pokemon
```jq
jq '.|length' pokemon.json
```

### 6. Write an expression to select the maximum value of Total
```jq
jq '[.[].Total] | max' pokemon.json
```

### 7. Write an expression to find the average value of Total
```jq
jq '[.[].Total] | add/length' pokemon.json
```

### 8. Select the Pokémon that have an Attack greater than 150
```jq
jq '.[] | select(.Attack > 150)' pokemon.json
```

### 9. Select only the Name and Total properties from Pokémon that have a Total greater than 650
```jq
jq '[.[] | select(.Total > 650) | {"Name", "Total"}]' pokemon.json
```

### 10. Select all the Pokémon that have a Type 2 value set
```jq
jq '[.[] | select(."Type 2" != null)][:5]' pokemon.json
```

### 11. Select the names of all the Legendary Pokemon
```jq
jq '[.[] | select(."Legendary" == true) | .Name]' pokemon.json 
```

### 12. Extract the names of all dual-type (having both Type 1 and Type 2 Pokemon)
```jq
jq '[.[] | select((."Type 1" != null) and (."Type 2" != null)) | .Name]' pokemon.json
```

### 13. Extract the names of all Poison-type Pokemon.
```jq
jq '.[] | select((."Type 1" == "Poison") or (."Type 2" == "Poison"))' pokemon.json
```

### 14. Calculate the average Defense of non-Legendary Pokemon
```jq
jq '[.[] | select(."Legendary" == false) | .Defense] | add/length' pokemon.json
```

### 15. Find the Pokémon with an HP greater than 70, sorted by HP in Ascending mode
```jq
 jq '[.[] | select( ."HP" > 70)] | sort_by(.HP)[:5]' pokemon.json
```

### 16. Find the Pokémon with a Defense value greater than 100, sorted by Defense in descending mode
```jq
jq '[ .[] | select(.Defense > 100) ] | sort_by(.Defense) | reverse | .[0:5]' pokemon.json
```

### 17. Identify the Pokémon with the highest Attack in Generation 2
```jq
jq '[.[] | select(.Generation == 2)] | sort_by(.Attack) | reverse | .[0]' pokemon.json 
```

### 18. Find all Defensive Pokémon (having more Defense than Attack)
```jq
jq '[ .[] | select(.Defense > .Attack)]' pokemon.json
```

### 19. Group the Pokémon by their primary type (Type 1).
```jq
jq '[.[] ]| group_by(."Type 1")' pokemon.json
```

### 20. Get the number of Pokémon by Generation
```jq
jq 'group_by(.Generation) | map({("\(.[0].Generation)"): (. | length) }) | add' pokemon.json
```

### 21. Get average HP by Type 1
```jq
jq 'group_by(."Type 1") | map({("\(.[0]."Type 1")"): ([.[].HP] | add / length) }) | add' pokemon.json
```

### 22. Use reduce to calculate how many Pokémon are legendary?
```jq
jq 'reduce .[] as $item (0; . + if $item.Legendary then 1 else 0 end)' pokemon.json

```