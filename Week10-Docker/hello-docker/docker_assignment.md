## Part 1.Mechanics

## Task 1.1 — Change the Python version

(base) [anna\@Anna](mailto:anna@Anna){.email} hello-docker % cat \>
hello.py \<\< 'EOF' import sys print(f"Hello from Python
{sys.version_info.major}.{sys.version_info.minor} inside a container!")
EOF docker build -t hello-docker . docker run --rm hello-docker

```         
[+] Building 0.8s (9/9) FINISHED                                                                                                                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                   0.0s
 => => transferring dockerfile: 144B                                                                                                                                                                   0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim                                                                                                                                     0.7s
 => [internal] load .dockerignore                                                                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                                                                        0.0s
 => [1/4] FROM docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f53f144965f755599aab1acda1e13cf1731b1b                                                                               0.0s
 => => resolve docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f53f144965f755599aab1acda1e13cf1731b1b                                                                               0.0s
 => [internal] load build context                                                                                                                                                                      0.0s
 => => transferring context: 144B                                                                                                                                                                      0.0s
 => CACHED [2/4] WORKDIR /app                                                                                                                                                                          0.0s
 => CACHED [3/4] RUN pip install pandas==2.2.3                                                                                                                                                         0.0s
 => [4/4] COPY hello.py .                                                                                                                                                                              0.0s
 => exporting to image                                                                                                                                                                                 0.0s
 => => exporting layers                                                                                                                                                                                0.0s
 => => exporting manifest sha256:0a92965c8fb0d753483eddafa7b5e29fe92a9129536c51ecc495b326e7b03c89                                                                                                      0.0s
 => => exporting config sha256:876a89329e187ddeb46f7391b32496fb0830167611b8fc6a0f1fdccba2cb65b6                                                                                                        0.0s
 => => exporting attestation manifest sha256:13eeb1be9eaa0ca4908a6423e8a7b59fcff62a62681e51f8febb15bdd51a655b                                                                                          0.0s
 => => exporting manifest list sha256:b06082e78b9664d1da461143d0bedec5a8706a3552fbf2c8f52bbd26b2157e6d                                                                                                 0.0s
 => => naming to docker.io/library/hello-docker:latest                                                                                                                                                 0.0s
 => => unpacking to docker.io/library/hello-docker:latest                                                                                                                                              0.0s
Hello from Python 3.9 inside a container!
```

## Task 1.2 — Break and fix the Dockerfile

(base) [anna\@Anna](mailto:anna@Anna){.email} hello-docker % cat \>
hello.py \<\< 'EOF' import sys, pandas print(f"Python
{sys.version_info.major}.{sys.version_info.minor}, pandas
{pandas.\_\_version\_\_}") EOF (base)
[anna\@Anna](mailto:anna@Anna){.email} hello-docker % docker build -t
hello-docker . docker run --rm hello-docker

```         
[+] Building 0.8s (8/8) FINISHED                           docker:desktop-linux
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 114B                                       0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim         0.7s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [1/3] FROM docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338  0.0s
 => => resolve docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338  0.0s
 => [internal] load build context                                          0.0s
 => => transferring context: 150B                                          0.0s
 => CACHED [2/3] WORKDIR /app                                              0.0s
 => [3/3] COPY hello.py .                                                  0.0s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => exporting manifest sha256:40747a80350b27fcf2691a2838af5e97497ee838  0.0s
 => => exporting config sha256:289888bdd4d4da4bc2a7bd050e94934ff1aeae9069  0.0s
 => => exporting attestation manifest sha256:6aa2ba7d70eb07c0d0674a5912a6  0.0s
 => => exporting manifest list sha256:4c1536b57bb1e13124abb6f5aaeb7e4ec34  0.0s
 => => naming to docker.io/library/hello-docker:latest                     0.0s
 => => unpacking to docker.io/library/hello-docker:latest                  0.0s
Traceback (most recent call last):
  File "/app/hello.py", line 1, in <module>
    import sys, pandas
ModuleNotFoundError: No module named 'pandas'

What's next:
    Debug this container error with Gordon → docker ai "help me fix this container error"
```

(base) [anna\@Anna](mailto:anna@Anna){.email} hello-docker % cat \>
Dockerfile \<\< 'EOF' FROM python:3.9-slim WORKDIR /app RUN pip install
pandas==2.2.3 COPY hello.py . CMD ["python", "hello.py"] EOF (base)
[anna\@Anna](mailto:anna@Anna){.email} hello-docker % docker build -t
hello-docker . docker run --rm hello-docker

```         
[+] Building 9.7s (9/9) FINISHED                           docker:desktop-linux
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 144B                                       0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim         0.5s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [1/4] FROM docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338  0.0s
 => => resolve docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338  0.0s
 => [internal] load build context                                          0.0s
 => => transferring context: 29B                                           0.0s
 => CACHED [2/4] WORKDIR /app                                              0.0s
 => [3/4] RUN pip install pandas==2.2.3                                    5.9s
 => [4/4] COPY hello.py .                                                  0.0s 
 => exporting to image                                                     3.2s 
 => => exporting layers                                                    2.6s 
 => => exporting manifest sha256:d538cf7d9e6777a4654d8a423cf91697a52fec98  0.0s 
 => => exporting config sha256:1c62e89e9d6548bfd08567e02554bfe13246318857  0.0s 
 => => exporting attestation manifest sha256:30922704b89a4ada8d21b1395c73  0.0s 
 => => exporting manifest list sha256:f4af4aadfef953d62a424322e4373c30a0c  0.0s
 => => naming to docker.io/library/hello-docker:latest                     0.0s
 => => unpacking to docker.io/library/hello-docker:latest                  0.6s
Python 3.9, pandas 2.2.3
```

## Final Dockerfile

(base) [anna\@Anna](mailto:anna@Anna){.email} hello-docker % cat
Dockerfile

```         
FROM python:3.9-slim
WORKDIR /app
RUN pip install pandas==2.2.3
COPY hello.py .
CMD ["python", "hello.py"]
```

## Part 2.Reproducibility judgment

Answer in 3–5 sentences each.

Question 2.1 — Why pin? In Task 1.2 you pinned a specific version of
pandas. Suppose you had instead written RUN pip install pandas (no
version). Your Dockerfile would still build and run today. Why is this a
reproducibility problem? What concretely could go wrong, and when?
Writing RUN pip install pandas without a version always installs the
latest available version at build time. This is a reproducibility
problem because a newer pandas version released in the future may have
breaking API changes or different default behaviours. Someone trying to
reproduce some work a year later might get different version, and the
script could fail or produce different output.

Question 2.2 — Recipe or cake? If you want a colleague to reproduce your
work, you can either send them the built image (e.g. via Docker Hub) or
send them only the Dockerfile and hello.py. Which is better for
reproducible research, and why? Give one concrete reason beyond "easier"
or "faster." Sharing the Dockerfile and source files is better for
reproducible research. The reason is transparency and auditability -
anyone can read the Dockerfile and understand exactly what environment
was built and why. A pre-built image can be run, but you cannot verify
what's inside or whether it was built from the code you think it was.
