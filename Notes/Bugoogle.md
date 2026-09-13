---
categories:
  - "[[Projects]]"
topics: []
created:
---


```shell
cd ~/rootandbeer/bugoogle
# use venv if needed
pip install -r indexer/requirements.txt
python3 -m indexer.main -c indexer/config.yaml
cp frontend/data/index.json hugo-site/static/data/
cp frontend/data/alerts.xml hugo-site/static/data/
```

```shell
export NVD_API_KEY=1bb0e328-8b28-4a98-8cf0-01a4ef1a10a4
export GITHUB_TOKEN=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
```

