হ্যাঁ 👍 এখন আমরা **PR-based CI/CD** শিখব। এটা real-world workflow-এর খুব common pattern।

আমাদের target হবে:

```text
feature branch
      ↓
Pull Request → main
      ↓
CI runs
      ↓
CI ✅
      ↓
Merge PR
      ↓
main updated
      ↓
CD runs
      ↓
EC2 deployed 🚀
```

### Step 24 — নতুন feature branch তৈরি

আপনার local repository root-এ আছেন কিনা নিশ্চিত করে চালান:

```bash id="5m0c2a"
git switch -c feature/test-cicd
```

Expected:

```text id="8h1p4n"
Switched to a new branch 'feature/test-cicd'
```

**বাংলা:** এবার সরাসরি `main`-এ change না করে feature branch-এ কাজ করব। এটাই PR workflow-এর শুরু।

এখন শুধু command-টা চালান এবং **done** বলুন।
