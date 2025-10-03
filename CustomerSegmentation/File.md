output_lines = []

for line in raw_string.splitlines():
    if line.startswith('\t\t') and ' as ' in line and ' with ' in line:
        # keep your existing attr if you need it elsewhere
        # match = re.search(r'\b[\w]+(?:\.[\w]+)*\b', line); attr = match.group()

        # ✅ NEW: grab the alias between "as" and "with"
        m_alias = re.search(r"\bas\s+([A-Za-z_][\w$]*)\s+with\b", line)
        if not m_alias:
            output_lines.append(line); continue

        col = m_alias.group(1)  # this should match df.index (COLUMN_NAME without prefix)

        if col in df.index:
            # business_term = df.loc[col, 'TERM_NAME']  # if you also rename alias, keep this
            business_deff = str(df.loc[col, 'TERM_BUSINESS_DEFINITION']).replace("'", "''")

            # replace the comment text with business definition (first occurrence)
            line = re.sub(
                r"(comment\s*=\s*)(['\"]).*?\2",
                lambda mm: f"{mm.group(1)}'{business_deff}'",
                line,
                count=1
            )

    output_lines.append(line)

output_str = '\n'.join(output_lines)
