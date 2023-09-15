
下载tomcat镜像：
```bash
docker pull tomcat
# Using default tag: latest
# latest: Pulling from library/tomcat
# 3f94e4e483ea: Pull complete 
# 857412f02e8d: Pull complete 
# aed87efb612a: Pull complete 
# b61ea314f37d: Pull complete 
# 5b0c06eed724: Pull complete 
# b756ef406e21: Pull complete 
# c9d2bc0ee37d: Pull complete 
# Digest: sha256:68c75d6f643ba2d68d145255e7e8d82ff90960bd2f33507a94d35a44c33007bf
# Status: Downloaded newer image for tomcat:latest
# docker.io/library/tomcat:latest
```

`overlay2/l/`  中的内容：
```bash
2AVULGMGQLYQGS4ICVMR2TBQE7 -> ../3dd5067524f9a656cfd27a73e83f713a36fd2e5ba5342084a7bc46572f7cd719/diff
3XP5WTKIX4OWJGMNJQH4GSSOV2 -> ../5c0186e4a2392c45dc5d4adfad913b2426defc47a1d67fe62a48a865a58e0b2c/diff
42SQETJ6YBWFWPZWIEAZOADHCV -> ../3c7f8306886b1174ccad17973a07eb27923888b12d444eaf59d23b1977d6f0f7/diff
7B6JJL4Y3K5DYVOF56Y4OQ7O62 -> ../41a0c71cd1a73fc0a197ced5d0cb1fc7b191a4d018c7ee4aaf45810e3fae3f1a/diff
A2X3PY27FLAEOSKIOUC54N42U6 -> ../f72d1aaba3e2dbfb0877b212cda7f75b1e0379ead8e5c2872fcaede460c95698/diff
TTXRF67WQ67LTR7URQHISTHSPS -> ../2ed54819f13d533f59fb741cd3e8feb004da33d380070b5d590e01eb83ca5e20/diff
UWQKXP5JHDMO4JXCLXU57RC4D3 -> ../2e4d78904c161b8422d221acfaefa62e7c842720ced0d5dda0943a4d0f18a80c/diff
```

# 运行tomcat
```
docker run -d -p 8080:8080 --name tomcat1 tomcat
```

变化：
```text
2AVULGMGQLYQGS4ICVMR2TBQE7 -> ../3dd5067524f9a656cfd27a73e83f713a36fd2e5ba5342084a7bc46572f7cd719/diff      
3HCNMZCB2GBZF4KN5O3BAQGDZO -> ../3de8bca5e193d0cca5bd9b0afb12da13520b11c14a712e18a9406767c75aadc9-init/diff(新)
3XP5WTKIX4OWJGMNJQH4GSSOV2 -> ../5c0186e4a2392c45dc5d4adfad913b2426defc47a1d67fe62a48a865a58e0b2c/diff
42SQETJ6YBWFWPZWIEAZOADHCV -> ../3c7f8306886b1174ccad17973a07eb27923888b12d444eaf59d23b1977d6f0f7/diff
4F227IH5ZB3XYHUTGP27STIKCW -> ../3de8bca5e193d0cca5bd9b0afb12da13520b11c14a712e18a9406767c75aadc9/diff(新)
7B6JJL4Y3K5DYVOF56Y4OQ7O62 -> ../41a0c71cd1a73fc0a197ced5d0cb1fc7b191a4d018c7ee4aaf45810e3fae3f1a/diff
A2X3PY27FLAEOSKIOUC54N42U6 -> ../f72d1aaba3e2dbfb0877b212cda7f75b1e0379ead8e5c2872fcaede460c95698/diff
TTXRF67WQ67LTR7URQHISTHSPS -> ../2ed54819f13d533f59fb741cd3e8feb004da33d380070b5d590e01eb83ca5e20/diff
UWQKXP5JHDMO4JXCLXU57RC4D3 -> ../2e4d78904c161b8422d221acfaefa62e7c842720ced0d5dda0943a4d0f18a80c/diff
```






