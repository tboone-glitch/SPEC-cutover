# Cutover: Teradata to Snowflake

Build a one-page browser game in a single index.html (vanilla JS and CSS, no libraries). Style it like a dark data-engineering console with a monospace font.

## Screen
- Header: "Teradata to Snowflake cutover", ACUs left (start 20), and a 60-second countdown labeled "Days to Teradata renewal".
- A table with one row per legacy table and these columns: TABLE | TERADATA ROWS | SNOWFLAKE ROWS | DELTA | STATUS | ACTION.
- 8 rows: EDW.CUSTOMER, EDW.ACCOUNT, EDW.ORDERS, EDW.INVOICE, EDW.PAYMENT, EDW.CLAIM, EDW.INVENTORY, EDW.GL_ENTRY. Each gets a random Teradata row count from 100,000 to 5,000,000. SNOWFLAKE ROWS starts as "-".

## Rules
- Each row has a "Migrate" button (1 ACU). After clicking, fill in SNOWFLAKE ROWS and run reconciliation.
- 50% of tables pass: the counts match, DELTA shows 0, and STATUS shows a green "Reconciled".
- 50% fail with one random Teradata-to-Snowflake issue. STATUS turns amber, and the row shows the symptom and three fix buttons:

| Issue | Symptom shown | Correct fix |
|---|---|---|
| SET table duplicates | Snowflake has more rows (+0.03%). Log: "Teradata SET table silently dropped duplicate rows; Snowflake kept them" | Dedupe with QUALIFY ROW_NUMBER() |
| Case sensitivity | Counts match, but "WHERE status = 'Active' returns 0 rows in Snowflake vs 48,112 in Teradata" | Apply case-insensitive collation |
| CHAR padding | Counts match, but "JOIN on cust_code drops 12,904 rows; trailing spaces don't match" | RTRIM CHAR columns |
| Decimal precision | Counts match, but "SUM(amount) off by $1,284.17" | Map DECIMAL(18,2) to NUMBER(18,2), not FLOAT |
| Time zone | Counts match, but "MAX(updated_at) shifted +5h" | Load as TIMESTAMP_NTZ in UTC |

- Each fix button costs 1 ACU. The three buttons are the correct fix plus two others from the list, in random order. The correct fix turns the row green. A wrong fix shows "Still not reconciled" and the row stays amber.
- Win: all 8 rows green ("Cutover complete"). Lose: time or ACUs run out ("Cutover failed"). Show tables reconciled, ACUs spent and wrong fixes. Reload the page to play again.

## Done when
- The game works in the browser. Commit index.html to a new branch and open a pull request.
