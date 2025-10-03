# assume df_meta has columns: COLUMN_NAME, TERM_NAME, TERM_BUSINESS_DEFINITION
# quick lookups (works even if df_meta isn't indexed)
term_by_col = dict(zip(df_meta['COLUMN_NAME'], df_meta['TERM_NAME']))
def_by_col  = dict(zip(df_meta['COLUMN_NAME'], df_meta['TERM_BUSINESS_DEFINITION']))

output_lines = []
for line in ddl_str.splitlines():

    # only touch column lines
    if ' as ' in line and ' with ' in line:
        # ✅ get the alias (semantic column name) after "as"
        m = re.search(r"\bas\s+([A-Za-z_][\w$]*)\s+with\b", line)
        if not m:
            output_lines.append(line)
            continue

        col = m.group(1)  # this is your COLUMN_NAME in df_meta

        # (you said business term replacement already works; keeping it simple)
        if col in term_by_col:
            business_term = term_by_col[col]
            # replace alias with TERM_NAME
            line = re.sub(r"(\bas\s+)[A-Za-z_][\w$]*(\s+with\b)",
                          rf"\1{business_term}\2", line, count=1)

        # 🔁 replace comment='...' with TERM_BUSINESS_DEFINITION
        if col in def_by_col:
            business_def_sql = str(def_by_col[col]).replace("'", "''")
            if 'comment' in line:
                line = re.sub(
                    r"(comment\s*=\s*)(['\"]).*?\2",
                    lambda mm: f"{mm.group(1)}'{business_def_sql}'",
                    line,
                    count=1
                )

    output_lines.append(line)

updated_sql = "\n".join(output_lines)
