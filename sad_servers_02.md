Markdown
# 📊 Post-Mortem: Saskatoon — Counting IPs 🌐

## 📝 Problem
The task was to analyze a web server access log file located at `/home/admin/access.log`. Each line represents an HTTP request, starting with the requester's IP address. 🔍

The goal was to identify the **IP address with the most requests** (appearing exactly 482 times) and save it to `/home/admin/highestip.txt`. 🎯

---

## 🕵️ Investigation & Discovery
To find the top requester, I needed to isolate the first column (IPs), count the occurrences, and sort them.

### 🛠️ Execution Pipeline:
1. **`cut -d' ' -f1 access.log`**: Extracted the first column using space as a delimiter.
2. **`sort`**: Organized the IPs to prepare for counting.
3. **`uniq -c`**: Counted unique occurrences of each IP.
4. **`sort -nr`**: Sorted the counts numerically in reverse order (highest first).

**Command used to verify the target IP:**
```bash
cut -d' ' -f1 access.log | sort | uniq -c | sort -nr | grep 482
Result:
482 66.249.73.135 ✅

🚀 Solution
After identifying the IP 66.249.73.135, I saved it to the required file:

Bash
echo "66.249.73.135" > /home/admin/highestip.txt
🧪 Verification
To ensure the result was correct, I performed two checks:

Grep Count:
grep -c -F -f /home/admin/highestip.txt /home/admin/access.log
Result: 482 📈

SHA1 Checksum:
sha1sum /home/admin/highestip.txt
Result: 6ef426c40652babc0d081d438b9f353709008e93 🔐 (Match confirmed!)

💻 Full History of Key Commands
Bash
# Analyze and find the most frequent IP
cut -d' ' -f1 access.log | sort | uniq -c | sort -nr | head -n 1

# Save the solution
echo "66.249.73.135" > /home/admin/highestip.txt

# Final validation
sha1sum /home/admin/highestip.txt
💡 Key Takeaways
Pipelining: Combining cut, sort, and uniq -c is a powerful way to perform frequency analysis in Linux. ⚡

Verification: Using sha1sum is a great way to validate data integrity without needing to see the raw solution.

Efficient Filtering: sort -nr is essential when you need the "top" results of a count. 🔝

Estimated Time to Solve: 15 minutes ⏳
