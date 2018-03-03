---
title:  "Tradeoffs between Normalization and Denormalization"
date:   2018-03-03 09:00:00
category: til
tags: [normalization, normalized, denormalization, denormalized, data, tables, sql, databases, vs, tradeoffs]
---

TIL the difference between normalized and denormalized schemas for modeling data, and some of the tradeoffs with each.

## Normalization

[Normalization][norm]{:target="_blank"} is a way of defining your database schema in a way that is optimized for fast and high integrity `writes` by ensuring no redundant data across tables.

The standard for normalization is to shoot for `3rd normal form` (3nf).

A brief recap of the criteria for achieving 3nf:

- Records in a table should contain primary_keys that uniquely reference them (1nf)

- There shouldn't be any repeated columns in a table (1nf)

- There shouldn't be any functionally dependent keys (2nf)

- There shouldn't be any transitvely functionally dependent keys (3nf)

Since tables don't contain any redundant data, the `storage` required for new records to any table is relatively small.

Since writes are small, they are also `fast`.

Writes are also guaranteed to leave database in a `consistent` state, due to `referential integrity` guarantees from foreign key constraints between related tables.

## Denormalization

[Denormalization][denorm]{:target="_blank"} is the act of adding redundancies or derived values in to your schema to optimize for `reads` that would otherwise be expensive in a normalized schema.

Think about a fully normalized database for a minute...

Everything is in its own table...

Everything has its own primary_key...

References between tables are maintained by foreign_key constraints...

... this is great from a storage and integrity perspective, but it can lead to **pieces of a query being distributed across many tables**, which leads to slow and complex joins to get the full picture for a query.

If you are able to anticipate the types of queries that your users might be making, **it could make sense to store some values redundantly in your system** to speed up query performance.

Adding denormalized values makes inserts and updates trickier: you need to ensure that denormalized values are properly maintained, since the integrity of these values are not automatically enforced by the schema.

The choice to denormalize should be made consciously. It should be documented, tested, and should be communicated across a team so all are aware of this additional consideration when writing to denormalized tables.

## Normalized Example

I'm building an app called `CashBackHero`, which involves modeling relationships between credit_cards and users through an entity called a wallet. A 3nf `normalized` schema for these entities might look something like this:

#### CashBack

id | value   |
---| ----------------- |
1  | 3                 |
2  | 4                 |
3  | 5                 |

#### Cards

id | name     | cash_back_id |
---| -------- | ------------------ |
1  | Chase    | 2                  |
2  | Discover | 1                  |
3  | Amex     | 3                  |

#### Wallets

id | name              |
---| ----------------- |
1  | Brian's Wallet    |
2  | Alex's Wallet     |
3  | Lauren's Wallet   |

#### CardWallets (associations between wallets and cards)

id | wallet_id     | card_id     |
---| ------------- | ----------- |
1  | 1             | 1           |
2  | 1             | 3           |
3  | 2             | 2           |
4  | 3             | 2           |
5  | 3             | 1           |

The tables above are `normalized`, since they do not contain any redundant data. Relationships between tables are maintained through `foreign_key` constraints to other tables. This stores data in its most `compact` form.

The lack of redundant data also optimizes for `writes` to the database. The most frequent writes are additions and removals to the `CardWallets` table, which just involves id columns which contain references to other information-containing tables.

Normalizing data also makes updates a breeze since **each table is a single source of truth for the information contained within.**

A normalized database also optimizes for **some kinds of reads,** like surfacing a list of all of the values in a particular table (like getting all of the cards).

Reads where data lies across multiple tables, though, become more challenging. A query to the normalized schema above to get the names and cash back values of cards for wallet 1 involves joining the `Wallets` and `Cards` tables to the `CardWallets` table, then joining the `CashBackValue` table to the `Cards` table. If these tables become large, queries  **could become slow** when compared to a denormalized version where all of the data lives in one table.

Speaking of denormalized...

## Denormalized Example

Check out what the normalized schema presented above would look like if it were fully denormalized:

#### CashBackSchema

id | card_name     | wallet_name | cash_back_value |
---| ------------- | ------------------- | --------------- |
1  | Chase         | Brian's Wallet      | 4               |
2  | Amex          | Brian's Wallet      | 5               |
3  | Discover      | Alex's Wallet       | 3               |
4  | Discover      | Lauren's Wallet     | 3               |
5  | Chase         | Lauren's Wallet     | 4               |

That's one monster table!

Ok... it's only 5 rows... but you get the idea that a denormalized table can often be easier to read from, since all of the data exists in one place.

Instead of needing to join 4 tables together to get the cards and cash_back_value for a user's wallet, we can now just execute one select statement to the `CashBackSchema` table above.

Querying for unique cards becomes a bit slower, since the entire table needs to be read to determine unique card_names.

Updates are also a bit trickier: we need to ensure integrity of our data `logically`. For example, if we want to update the cash_back_value of the Chase card in Brian's Wallet, we also need to consider if we need to update the cash_back_value of the Chase card in Lauren's wallet as well. **Updates need to be made in multiple places**, which can be slow for large tables.

In a normalized schema, updates to something like "wallet_display_name" can just be made in one place (the `Wallets` table), instead of needing to comb through the entire `CashBackSchema` to ensure every row with the name "Brian's Wallet" is updated appropriately.

[norm]: https://technet.microsoft.com/en-us/library/ms191178(v=sql.105).aspx

[denorm]: https://msdn.microsoft.com/en-us/library/cc505841.aspx

