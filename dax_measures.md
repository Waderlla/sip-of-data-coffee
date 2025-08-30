### ============================================================================
### DAX Measures — Sip of Data: Stories Over Coffee
### File: dax_measures.txt
## Description
### Collected measures and calculated objects used in the Power BI model.
## Notes
### - All % measures follow the same safe pattern: DIVIDE(licznik, mianownik, 0)
### - The base population for % is coffee drinkers with non-blank answers in the target column.
### - Disconnected tables: CupChoices (categories/order) and CupAxis (single-item axis).
### ============================================================================

### ---------------------------------------------------------------------------
### 1) Base population
### ---------------------------------------------------------------------------
```DAX
drinking coffee = 
CALCULATE(
    DISTINCTCOUNT(coffee_survey_clean[Index]),
    coffee_survey_clean[Do you drink coffee?] = "Yes"
)
```



### ---------------------------------------------------------------------------
### 2) % MEASURES — ADDITIONS, PREPARATION METHODS, STRENGTH, etc.
### Pattern: replace COLUMN and TARGET with appropriate values.
### ---------------------------------------------------------------------------
## Template
### % (Label) :=
### VAR licznik =
### CALCULATE(
### [drinking coffee],
### REMOVEFILTERS( coffee_survey_clean[COLUMN] ),
### coffee_survey_clean[COLUMN] = "TARGET"
### )
### VAR mianownik =
### CALCULATE(
### [drinking coffee],
### REMOVEFILTERS( coffee_survey_clean[COLUMN] ),
### coffee_survey_clean[COLUMN] <> BLANK()
### )
### RETURN DIVIDE(licznik, mianownik, 0)

### -------------------- Additions --------------------
```DAX
% (milk) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] =
            "Milk or cream (dairy)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```



```DAX
% (Plant-based) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] =
            "Plant-based milk (e.g., soy, oat, almond)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (sugar) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] =
            "Sugar / sweetener"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```



```DAX
% (syrup) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] =
            "Flavored syrup (vanilla, caramel, etc.)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```



```DAX
% (spices) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] =
            "Spices (e.g., cinnamon, cardamom)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (no_additives) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] =
            "Nothing – I drink it black (without additives)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (other) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] =
            "Other"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[What do you usually add to your coffee?]),
        coffee_survey_clean[What do you usually add to your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```



### -------------------- Strength preference --------------------
```DAX
% (Definitely strong) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] =
            "Definitely strong (very intense flavor / high caffeine content)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (Rather strong) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] =
            "Rather strong"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] <> BLANK()  
    )
RETURN DIVIDE(licznik, mianownik, 0)
```

```DAX
% (Rather mild) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] =
            "Rather mild"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```



```DAX
% (Definitely mild) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] =
            "Definitely mild (light flavor or low caffeine content)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS(coffee_survey_clean[Do you prefer your coffee strong or mild?]),
        coffee_survey_clean[Do you prefer your coffee strong or mild?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```



### -------------------- Preparation method --------------------
```DAX
% (Traditional boiled coffee) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] =
            "Traditional boiled coffee (“Turkish style” or grounds brewed directly)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] <> BLANK() 
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (Instant coffee) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] =
            "Instant coffee (soluble – with hot water)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] <> BLANK()  
    )
RETURN DIVIDE(licznik, mianownik, 0)
```

```DAX
% (I buy ready-made coffee) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] =
            "I buy ready-made coffee (don’t brew it myself, e.g., from a café or vending machine)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (French press) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] =
            "French press / moka pot (plunger pot or moka pot)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (Espresso machine) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] =
            "Espresso machine (manual or automatic) – e.g., at home or from a barista in a café"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (Cold brew) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] =
            "Cold brew (brewed cold)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] <> BLANK()       )
RETURN DIVIDE(licznik, mianownik, 0)
```


```DAX
% (capsules) = 
VAR licznik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] =
            "Capsules / pods (capsule machine such as Nespresso, Senseo, etc.)"
    )
VAR mianownik =
    CALCULATE(
        [drinking coffee],
        REMOVEFILTERS('coffee_survey_clean'[How do you usually prepare or get your coffee?]),
        'coffee_survey_clean'[How do you usually prepare or get your coffee?] <> BLANK()   
    )
RETURN DIVIDE(licznik, mianownik, 0)
```


### ---------------------------------------------------------------------------
### 3) Disconnected Slicer: Cups per day
### ---------------------------------------------------------------------------

### Calculated column to align fact values with user-facing labels
```DAX
CupsPerDay_label = 
SWITCH(
    coffee_survey_clean[How many cups of coffee do you usually drink per day?],
    "1",         "1 cup",
    "2",         "2 cups",
    "3–4",       "3-4 cups",
    "5 or more", "5 or more",
    BLANK()
)
```


### Disconnected table with explicit sort order
```DAX
CupChoices =
DATATABLE(
  "CupsPerDay", STRING,
  "SortOrder", INTEGER,
  {
    {"1 cup", 1},
    {"2 cups", 2},
    {"3-4 cups", 3},
    {"5 or more", 4}
  }
)
```


### Helper: selected label from slicer (with default)
```DAX
Selected cup label =
COALESCE(
    SELECTEDVALUE( CupChoices[CupsPerDay] ),
    "1 cup"
)
```


### Optional single-category axis table (for visuals that require an X axis)
```DAX
CupAxis = 
DATATABLE(
    "Axis", STRING,
    { {"Selected"} }
)
```


### Respondents base (nonblank cups)
```DAX
Respondents base (nonblank cups) = 
CALCULATE(
    DISTINCTCOUNT( coffee_survey_clean[Index] ),
    KEEPFILTERS( NOT ISBLANK( coffee_survey_clean[How many cups of coffee do you usually drink per day?] ) )
)
```

### Respondents (selected cup)
```DAX
Respondents (selected cup) = 
VAR cup = SELECTEDVALUE( CupChoices[CupsPerDay], "1 cup" )
RETURN
CALCULATE(
    DISTINCTCOUNT( coffee_survey_clean[Index] ),
    KEEPFILTERS( NOT ISBLANK( coffee_survey_clean[CupsPerDay_label] ) ),
    TREATAS( { cup }, coffee_survey_clean[CupsPerDay_label] )
)
```
### Respondents % (selected cup)
```DAX
Respondents % (selected cup) = 
DIVIDE(
    [Respondents (selected cup)],
    [Respondents base (nonblank cups)],
    0
)
```

### ============================================================================
### END OF FILE
### ============================================================================