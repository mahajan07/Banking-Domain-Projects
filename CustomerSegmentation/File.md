
# Assumptions:
# - raw_string: the semantic view DDL (multiline string)
# - df: pandas DataFrame with index = COLUMN_NAME (no EOSCAR_ACDV. prefix)
# - df has a column 'TERM_BUSINESS_DEFINITION'
#
# This keeps your original line-by-line flow and DOES NOT delete other text.

import re

alias_re = re.compile(r"\bas\s+([A-Za-z_][\w$]*)\s+with\b")  # alias (COLUMN_NAME) between "as" and "with"
comment_sq = re.compile(r"(comment\s*=\s*)'(?:[^']|'')*'")   # comment='...'
comment_dq = re.compile(r'(comment\s*=\s*)"(?:[^"]|"")*"')   # comment="..."

output_lines = []

for line in raw_string.splitlines():
    if (' as ' in line) and (' with ' in line) and ('comment' in line):
        m = alias_re.search(line)
        if m:
            col = m.group(1)  # matches your df index (COLUMN_NAME without schema)
            if col in df.index:
                # fetch and SQL-escape the definition
                business_def = str(df.loc[col, 'TERM_BUSINESS_DEFINITION']).strip().replace("'", "''")
                # replace ONLY the quoted string after comment= (won't touch the rest of the line)
                new_line = comment_sq.sub(lambda mm: f"{mm.group(1)}'{business_def}'", line, count=1)
                new_line = comment_dq.sub(lambda mm: f"{mm.group(1)}'{business_def}'", new_line, count=1)
                output_lines.append(new_line)
                continue
    output_lines.append(line)

output_str = "\n".join(output_lines)
# If you need a final SQL wrapper like your original:
# sql_query = f""" {output_str.strip()} """
print(output_str)
