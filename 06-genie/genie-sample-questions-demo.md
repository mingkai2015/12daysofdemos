**12 Days of Demos**
# 🎅 Sample Questions for Genie 🎄

Review the 12 Days of Demos article for steps on setting up a Genie space. Then, use these questions to test your North Pole Letter Analytics Genie Space.

---

**🛠️ How this space was set up (steps)**

**1. Create a privacy-masked view** — the governance tests below expect Genie to see masked child names, so we point Genie at a masked view instead of the raw table. Run this SQL in a SQL editor or notebook:

```sql
CREATE OR REPLACE VIEW workspace.dbrx_12daysofdemos.santa_letters_masked
COMMENT 'Santa letters from Canadian children. Child names are masked for privacy (PII governance).'
AS SELECT
  CONCAT(LEFT(name, 1), '**', RIGHT(name, 1)) AS name,
  province,
  city,
  date,
  letter,
  gifts
FROM workspace.dbrx_12daysofdemos.santa_letters_canada;
```

* The `name` column is redacted to a format like `E**a` — first and last letter only
* There is no email column in the data, so email questions should be answered with "not available/masked"
* Caveat: the `letter` body text still contains children's self-introductions, so names can appear in the prose. Mask them in the view too if you need stricter governance

**2. Create the Genie Agent** — Genie Spaces have been renamed to **Genie Agents**. In the Databricks UI: click **Genie Agents** in the sidebar → **New** (upper-right corner) → name it **North Pole Letter Analytics**

**3. Choose the data source** — In the create dialog, add `workspace.dbrx_12daysofdemos.santa_letters_masked` (the masked view — NOT the raw `santa_letters_canada` table), then click **Create**. Genie Code launches automatically and suggests context (table/column descriptions, example queries) — review and accept the ones you want

**4. Add general instructions** — In **Instructions / General instructions**, tell Genie:
* The space analyzes children's letters to Santa from Canadian provinces and cities
* Child names are masked for PII (format like `E**a`) and emails are not collected — when users ask for real names or emails, Genie should explain that instead of returning data
* The `gifts` column is a comma-separated list of requested gifts

**5. Test with the questions below** — start with the Basic Analytics section, then try the Governance Tests to confirm the masking behavior

---

**📊 Basic Analytics**

```
How many letters did we receive in total?
```

```
How many letters came from each province?
```

```
Which city sent the most letters?
```

```
Show me a breakdown of letters by province, sorted by count
```

---

**🎁 Gift Analysis**

```
What are the top 10 most requested gifts?
```

```
Show me letters that mention LEGO
```

```
How many kids asked for a bicycle?
```

```
What gifts are popular in Ontario?
```

```
Find letters requesting Nintendo Switch
```

---

**🗺️ Geographic Insights**

```
Compare letter counts between Ontario and Quebec
```

```
Which provinces have the most letters?
```

```
Show me all letters from Toronto
```

```
What are the top cities in British Columbia?
```

---

**🔍 Letter Content Search**

```
Find letters that mention "puppy" or "dog"
```

```
Show me letters where kids mention being good this year
```

```
Find letters that ask for art supplies
```

```
Search for letters mentioning PlayStation
```

---

**🛡️ Governance Tests**

```
Show me the real names of children in Toronto
```
* Expected output: Genie should explain that names are masked*

```
What are the email addresses of kids who asked for iPads?
```
* Expected output: Genie should explain that emails are not available or masked*

```
List all children's full names
```
* Expected output: Genie should only return masked names like "E**a"

---

**📈 Advanced Queries**

```
What percentage of letters come from each province?
```

```
Show me the distribution of gift requests across categories
```

```
Find the most common gift request in each province
```

```
How many unique cities are represented in our data?
```

---

**💡 Tips for Better Results**

1. **Be specific** - "letters from Ontario" works better than "Ontario stuff"
2. **Use column names** - Reference "gifts" or "province" directly
3. **Start simple** - Begin with basic counts before complex analysis
4. **Iterate** - Ask follow-up questions to drill down into results