FROM maven:3.9-eclipse-temurin-11

RUN apt-get update && apt-get install -y git patch && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace

RUN git clone https://github.com/apache/pinot.git /workspace/repo && cd /workspace/repo && \
    BASE=ff75a7dbc5563b1a43abd2323d891ddd9a7a58f9 && \
    cur_branch="$(git rev-parse --abbrev-ref HEAD)" && \
    git reset --hard "$BASE" && \
    git for-each-ref --format='%(refname:short)' refs/heads | while read -r b; do \
      [ "$b" = "$cur_branch" ] || git branch -D "$b"; \
    done && \
    git remote | xargs -r -n1 git remote remove && \
    rm -f .git/config.lock && \
    git reflog expire --expire=now --all && \
    git gc --prune=now --aggressive

WORKDIR /workspace/repo

RUN mvn install -pl pinot-common,pinot-core -am --no-transfer-progress -DskipTests 2>&1 | tail -50 || true

WORKDIR /workspace

CMD ["sleep", "infinity"]
