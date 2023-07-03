# Exploratory Cypher Queries

## Most valuable merchants

```sql
MATCH (t:Purchase)<-[:SELL]-(m:Merchant)
RETURN m.name, SUM(t.amount) AS total
ORDER BY SUM(t.amount) DESC
LIMIT 10
```

### Result

```raw
╒══════════════════╤══════════════════╕
│m.name            │total             │
╞══════════════════╪══════════════════╡
│"It Smart Group"  │3769957.383542169 │
├──────────────────┼──────────────────┤
│"Areon Impex"     │3583469.9180520033│
├──────────────────┼──────────────────┤
│"21st Century Fox"│3572767.081992541 │
├──────────────────┼──────────────────┤
│"Vodafone"        │3566854.7667955994│
├──────────────────┼──────────────────┤
│"Amazon.com"      │3540095.287769094 │
├──────────────────┼──────────────────┤
│"Demaco"          │3524523.1630317084│
├──────────────────┼──────────────────┤
│"Facebook"        │3517539.635354998 │
├──────────────────┼──────────────────┤
│"Zepter"          │3490776.4376936606│
├──────────────────┼──────────────────┤
│"ExxonMobil"      │3483800.2829920193│
├──────────────────┼──────────────────┤
│"Metro Cash&Carry"│3457534.249150458 │
```

## Customers with the most credit card transactions

```sql
MATCH (c:Customer)-[:HAS]->(cc:Credit_card)-[:BUY]->(p:Purchase)
RETURN 
    c.first_name + ' ' + c.last_name AS name,
    COUNT(p) AS total_purchase,
    SUM(p.amount) AS total_amount
ORDER BY COUNT(p) DESC 
LIMIT 10
```

### Result

```raw
╒═════════════════╤══════════════╤══════════════════╕
│name             │total_purchase│total_amount      │
╞═════════════════╪══════════════╪══════════════════╡
│"Dani Flanders"  │179           │1882640.2654660696│
├─────────────────┼──────────────┼──────────────────┤
│"Greta Swan"     │179           │1978438.3806692604│
├─────────────────┼──────────────┼──────────────────┤
│"Mark Jobson"    │179           │1740844.7296802902│
├─────────────────┼──────────────┼──────────────────┤
│"Janice Simmons" │178           │1680983.5578798803│
├─────────────────┼──────────────┼──────────────────┤
│"Candace Shea"   │178           │1814280.138332621 │
├─────────────────┼──────────────┼──────────────────┤
│"Matt Carter"    │178           │1801833.8113858304│
├─────────────────┼──────────────┼──────────────────┤
│"Hayden Garcia"  │177           │1756431.3128474005│
├─────────────────┼──────────────┼──────────────────┤
│"Rebecca Corbett"│177           │1763501.33452983  │
├─────────────────┼──────────────┼──────────────────┤
│"Rowan Harper"   │177           │1763699.88887497  │
├─────────────────┼──────────────┼──────────────────┤
│"Morgan James"   │177           │1742265.338604869 │
└─────────────────┴──────────────┴──────────────────┘
```

## Customers with similar shopping interests in 2021

```sql
MATCH 
    (subject:Customer {first_name: 'Peter', last_name: 'Bailey'}),
    (subject)-[:HAS]->()-[:BUY]->()<-[:SELL]-(merchant:Merchant),
    (person:Customer)-[:HAS]->()-[:BUY]->(purchase:Purchase)<-[:SELL]-(merchant)
WHERE
    purchase.datetime.year = 2021
RETURN
    person.first_name + ' ' + person.last_name AS full_name,
    COUNT(merchant) AS score,
    COLLECT(DISTINCT merchant.name) AS merchants
ORDER BY score DESC
LIMIT 10
```

### Result

```raw
╒═════════════════╤═════╤══════════════════════════════════════════════════════════════════════╕
│full_name        │score│merchants                                                             │
╞═════════════════╪═════╪══════════════════════════════════════════════════════════════════════╡
│"Greta Swan"     │569  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Janice Simmons" │564  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Candace Shea"   │563  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Rebecca Corbett"│547  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Dani Flanders"  │540  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Hayden Garcia"  │539  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Morgan James"   │539  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Matt Carter"    │532  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Erick Ingram"   │526  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
├─────────────────┼─────┼──────────────────────────────────────────────────────────────────────┤
│"Rowan Harper"   │522  │["Mars", "Global Print", "Facebook", "ExxonMobil", "Biolife Grup", "Le│
│                 │     │adertech Consulting", "Amazon.com", "Coca-Cola Company", "DynCorp", "T│
│                 │     │elekom", "21st Century Fox", "CarMax", "Apple Inc.", "Team Guard SRL",│
│                 │     │ "Comodo", "It Smart Group", "Areon Impex", "Zepter", "Boeing", "BuzzF│
│                 │     │eed", "Metro Cash&Carry", "Danone", "Demaco", "Vodafone", "AECOM", "Er│
│                 │     │ickson", "UPC", "ENEL"]                                               │
└─────────────────┴─────┴──────────────────────────────────────────────────────────────────────┘

```