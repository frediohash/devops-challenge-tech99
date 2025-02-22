1. Create file .sh with this command : 
vi fetch_tsla_orders.sh

2. Make the file executable, run with this command :
chmod +x fetch_tsla_orders.sh

3. Fill fetch_tsla_orders.sh with this :
#!/bin/bash
grep '"symbol": "TSLA"' ./transaction-log.txt | grep '"side": "sell"' | awk -F'"' '{print $4}' | xargs -I {} curl -s https://example.com/api/{}
grep '"symbol": "TSLA"' ./transaction-log.txt | grep '"side": "sell"' | awk -F'"' '{print $4}' | xargs -I {} curl -s https://example.com/api/{} > ./output.txt

4. Run script with this :
sh fetch_tsla_orders.sh

