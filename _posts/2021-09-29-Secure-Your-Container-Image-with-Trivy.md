   ![trivylogo](https://user-images.githubusercontent.com/106372696/171162739-5ba01020-781d-440d-8341-3ccef375d31c.png)


Hi Everyone,

Today We're going to see secure your container images with trivy scanner. Before we get started let's see what trivy is and its functionallity.

#### Trivy:- Introduction
According to the official documentation from Aquasecurity **Trivy** (tri pronounced like **tri**gger, 'vy' pronounced like en**vy**) is a simple and comprehensive scanner for vulnerabilities in container images, file systems, and Git repositories, as well as for configuration issues. 

![trivy-workflow](https://user-images.githubusercontent.com/106372696/171162703-853344cf-fb91-42bc-b1f1-3e730470c239.png)


**Trivy** detects vulnerabilities of OS packages (Alpine, RHEL, CentOS, etc.) and language-specific packages (Bundler, Composer, npm, yarn, etc.). In addition, 'Trivy' scans Infrastructure as Code (IaC) files such as Terraform, Dockerfile and Kubernetes, to detect potential configuration issues that expose your deployments to the risk of attack. 'Trivy' also scans hardcoded secrets like passwords, API keys and tokens. 'Trivy' is easy to use. Just install the binary and you're ready to scan.

#### Trivy:- Features

1. Easy to Install
	-  You can install Trivy on different os like Linux, Mac.
	
    Linux :
	
    ```bash
	  $ apt-get install trivy
  
		  	or
      
	  $ yum install trivy
	  ```

    Mac :
  
    ```bash
	  $ brew install trivy
    ```

2. Easy to Use
	- Just specify the Image name, Config files, filesystem directory and remote git directory. You can also save your image locally then use trivy.  

3. Detect Comprehensive vulnerability

	-   You can detect vulnerabilities in the os packages this includes with Alpine Linux, Red Hat Universal Base Image, Red Hat Enterprise Linux, CentOS, AlmaLinux, Rocky Linux, CBL-Mariner, Oracle Linux, Debian, Ubuntu, Amazon Linux, openSUSE Leap, SUSE Enterprise Linux, Photon OS and Distroless.
	-   Language-specific packages (Bundler, Composer, Pipenv, Poetry, npm, yarn, Cargo, NuGet, Maven, and Go)

4. Helps DevSecOps

	- Easy to Integrate with CI/CD pipeline to identify potential vulnerablities within the container images. Suitable for Continious integration like Jenkins, Gitlab CI, etc. 

5. Speed

 	- Scan the image within 10 seconds (this one also depends on your network speed and Image size.) and Consequent scans will finish in single seconds.

#### Trivy :- Scan Images

1. Scan Docker Images

	- First we're going to see how to scan docker images. In my case I used alpine for scanning (alpine is a light weight linux image), you can see within a second trivy find different vulnerablities in alpine.

```bash
$ trivy i alpine:3.13.0                                                                                                                      
2021-09-29T15:58:36.865+0530    INFO    Detected OS: alpine
2021-09-29T15:58:36.865+0530    INFO    Detecting Alpine vulnerabilities...
2021-09-29T15:58:36.867+0530    INFO    Number of language-specific files: 0

alpine:3.13.0 (alpine 3.13.0)

Total: 45 (UNKNOWN: 0, LOW: 2, MEDIUM: 8, HIGH: 30, CRITICAL: 5)

┌──────────────┬────────────────┬──────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────────────┐
│   Library    │ Vulnerability  │ Severity │ Installed Version │ Fixed Version │                            Title                            │
├──────────────┼────────────────┼──────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ apk-tools    │ CVE-2021-36159 │ CRITICAL │ 2.12.0-r4         │ 2.12.6-r0     │ libfetch before 2021-07-26, as used in apk-tools, xbps, and │
│              │                │          │                   │               │ other products, mishandles...                               │
│              │                │          │                   │               │ https://avd.aquasec.com/nvd/cve-2021-36159                  │
│              ├────────────────┼──────────┤                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│              │ CVE-2021-30139 │ HIGH     │                   │ 2.12.5-r0     │ In Alpine Linux apk-tools before 2.12.5, the tarball parser │
│              │                │          │                   │               │ allows a buffer...                                          │
│              │                │          │                   │               │ https://avd.aquasec.com/nvd/cve-2021-30139                  │
├──────────────┼────────────────┼──────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
...

...

```

As I mentioned earlier you can save your image locally then use trivy for scanning.


 ```bash
 $ docker save alpine:3.13.0 -o aline-13.tar

 $ trivy i --input aline-13.tar                                                                                                                                   
2021-09-29T15:49:49.063+0530	INFO	Detected OS: alpine
2021-09-29T15:49:49.063+0530	INFO	Detecting Alpine vulnerabilities...
2021-09-29T15:49:49.066+0530	INFO	Number of language-specific files: 0

aline-13.tar (alpine 3.13.0)

Total: 45 (UNKNOWN: 0, LOW: 2, MEDIUM: 8, HIGH: 30, CRITICAL: 5)

┌──────────────┬────────────────┬──────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────────────┐
│   Library    │ Vulnerability  │ Severity │ Installed Version │ Fixed Version │                            Title                            │
├──────────────┼────────────────┼──────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ apk-tools    │ CVE-2021-36159 │ CRITICAL │ 2.12.0-r4         │ 2.12.6-r0     │ libfetch before 2021-07-26, as used in apk-tools, xbps, and │
│              │                │          │                   │               │ other products, mishandles...                               │
│              │                │          │                   │               │ https://avd.aquasec.com/nvd/cve-2021-36159                  │
│              ├────────────────┼──────────┤                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│              │ CVE-2021-30139 │ HIGH     │                   │ 2.12.5-r0     │ In Alpine Linux apk-tools before 2.12.5, the tarball parser │
│              │                │          │                   │               │ allows a buffer...                                          │
│              │                │          │                   │               │ https://avd.aquasec.com/nvd/cve-2021-30139                  │
├──────────────┼────────────────┼──────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤

...
...

```

2. Scan Remote Repository

```bash
$ trivy repo https://github.com/containers/skopeo   
Enumerating objects: 18900, done.
Counting objects: 100% (18900/18900), done.
Compressing objects: 100% (8809/8809), done.
Total 18900 (delta 10238), reused 16708 (delta 8638), pack-reused 0
2021-09-29T16:38:52.440+0530    INFO    Number of language-specific files: 1
2021-09-29T16:38:52.441+0530    INFO    Detecting gomod vulnerabilities...

go.mod (gomod)

Total: 23 (UNKNOWN: 0, LOW: 2, MEDIUM: 14, HIGH: 6, CRITICAL: 1)

┌─────────────────────────────┬──────────────────┬──────────┬─────────────────────────────────────────────┬───────────────────────────────────────────┬──────────────────────────────────────────────────────────────┐
│           Library           │  Vulnerability   │ Severity │              Installed Version              │               Fixed Version               │                            Title                             │
├─────────────────────────────┼──────────────────┼──────────┼─────────────────────────────────────────────┼───────────────────────────────────────────┼──────────────────────────────────────────────────────────────┤
│ github.com/dgrijalva/jwt-go │ CVE-2020-26160   │ HIGH     │ 3.2.0+incompatible                          │                                           │ jwt-go: access restriction bypass vulnerability              │
│                             │                  │          │                                             │                                           │ https://avd.aquasec.com/nvd/cve-2020-26160                   │
├─────────────────────────────┼──────────────────┼──────────┼─────────────────────────────────────────────┼───────────────────────────────────────────┼──────────────────────────────────────────────────────────────┤
│ github.com/miekg/dns        │ CVE-2019-19794   │ MEDIUM   │ 1.0.14                                      │ 1.1.25-0.20191211073109-8ebf2e419df7      │ golang-github-miekg-dns: predictable TXID can lead to        │
│                             │                  │          │                                             │                                           │ response forgeries                                           │
│                             │                  │          │                                             │                                           │ https://avd.aquasec.com/nvd/cve-2019-19794                   │
├─────────────────────────────┼──────────────────┼──────────┼─────────────────────────────────────────────┼───────────────────────────────────────────┼──────────────────────────────────────────────────────────────┤
│ go.etcd.io/etcd             │ CVE-2018-1098    │ HIGH     │ 0.5.0-alpha.5.0.20200910180754-dd1b699fc489 │ 3.4.0                                     │ etcd: Cross-site request forgery via crafted local POST      │
│                             │                  │          │                                             │                                           │ forms                                                        │
│                             │                  │          │                                             │                                           │ https://avd.aquasec.com/nvd/cve-2018-1098                    │
├─────────────────────────────┼──────────────────┼──────────┼─────────────────────────────────────────────┼───────────────────────────────────────────┼──────────────────────────────────────────────────────────────┤
│ go.etcd.io/etcd             │ CVE-2018-1099    │ MEDIUM   │ 0.5.0-alpha.5.0.20200910180754-dd1b699fc489 │ 3.4.0                                     │ etcd: DNS rebinding vulnerability in etcd server             │
│                             │                  │          │                                             │                                           │ https://avd.aquasec.com/nvd/cve-2018-1099                    │
├─────────────────────────────┼──────────────────┼──────────┼─────────────────────────────────────────────┼───────────────────────────────────────────┼──────────────────────────────────────────────────────────────┤
...
...

```

3. Scan Directory for Misconfiguration

```bash

$ ls build/
Dockerfile

$ trivy config ./build

```

4. Scan file system (credits : Aquasecurity)

```bash
$ trivy fs --security-checks vuln,secret,config myproject/

2021-07-09T12:03:27.564+0300    INFO    Number of language-specific files: 1
2021-07-09T12:03:27.564+0300    INFO    Detecting pipenv vulnerabilities...
2021-07-09T12:03:27.566+0300    INFO    Detected config files: 1

Pipfile.lock (pipenv)
=====================
Total: 1 (HIGH: 1, CRITICAL: 0)

+----------+------------------+----------+-------------------+---------------+---------------------------------------+
| LIBRARY  | VULNERABILITY ID | SEVERITY | INSTALLED VERSION | FIXED VERSION |                 TITLE                 |
+----------+------------------+----------+-------------------+---------------+---------------------------------------+
| httplib2 | CVE-2021-21240   | HIGH     | 0.12.1            | 0.19.0        | python-httplib2: Regular              |
|          |                  |          |                   |               | expression denial of                  |
|          |                  |          |                   |               | service via malicious header          |
|          |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-21240 |
+----------+------------------+----------+-------------------+---------------+---------------------------------------+

Dockerfile (dockerfile)
=======================
Tests: 23 (SUCCESSES: 22, FAILURES: 1, EXCEPTIONS: 0)
Failures: 1 (HIGH: 1, CRITICAL: 0)

+---------------------------+------------+----------------------+----------+------------------------------------------+
|           TYPE            | MISCONF ID |        CHECK         | SEVERITY |                 MESSAGE                  |
+---------------------------+------------+----------------------+----------+------------------------------------------+
| Dockerfile Security Check |   DS002    | Image user is 'root' |   HIGH   | Last USER command in                     |
|                           |            |                      |          | Dockerfile should not be 'root'          |
|                           |            |                      |          | -->avd.aquasec.com/appshield/ds002       |
+---------------------------+------------+----------------------+----------+------------------------------------------+

```

#### In the End..
Trivy is very usefull especially when it comes to DevSecOps during CI process this one is good practice to have in order to avoid security related issues (For developers, accidently commits API keys). You can find more details and usage about trivy on their official github repo.

[Trivy - AquaSecurity](https://github.com/aquasecurity/trivy)

  Poster credits : Aqua Security 
