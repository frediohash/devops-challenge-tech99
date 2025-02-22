1. Create file .sh with this command : 
vi fetch_tsla_orders.sh

2. Make the file executable, run with this command :
chmod +x fetch_tsla_orders.sh

3. Fill fetch_tsla_orders.sh with this :
#!/bin/bash

INPUT_FILE="./transaction-log.txt"
OUTPUT_FILE="./output.txt"
API_URL="https://example.com/api"
> "$OUTPUT_FILE"
awk -F'"' '/"symbol": "TSLA"/ && /"side": "sell"/ {print $4}' "$INPUT_FILE" | while read -r order_id; do
    curl -s "$API_URL/$order_id" >> "$OUTPUT_FILE"
    echo "" >> "$OUTPUT_FILE"  
done
echo "Finished processing. Results saved in $OUTPUT_FILE"


4. Run script with this :
sh fetch_tsla_orders.sh
