# CI/CD Pipeline with Docker and Github Actions v2

I am trying to use github actions to automate docker builds when a trigger to main branch happens. And also a correct and sudtainable tagging has implemented by both the tag with hash and a latest tag.

Therefore I have write the workflow file like below.

```
name: Build and Push Docker image to Docker Hub

on: push

jobs:
  push_to_registry:
    name: Push Docker image to Docker Hub
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repo
        uses: actions/checkout@v3
      
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
    
      - name: Set short SHA
        id: vars
        run: echo "SHORT_SHA=$(echo ${{ github.sha }} | cut -c1-7)" >> $GITHUB_ENV
    
      - name: Build and push Docker image
        uses: docker/build-push-action@v4
        with:
          context: ./
          push: true
          tags: |
                kalharacodes/githubactionstest:${{ env.SHORT_SHA }}
                kalharacodes/githubactionstest:latest
```

