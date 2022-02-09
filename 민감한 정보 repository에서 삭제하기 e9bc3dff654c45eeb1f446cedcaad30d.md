# 민감한 정보 repository에서 삭제하기

- Repository에서 실수나 뒤늦게 민감한 자료(pw 등)을 올리는 경우가 빈번할 것이다.
- 이때 branch 등을 유지하면서 민감한 파일, commit 내 history를 다른 의존관계 영향 없이 git 혹은 github 상에서 삭제할 수 있다

1. **BFG Repo Cleaner** 이용
    - BGF란? : git-fill-branch를 대신할 수 있으며(**심지어 더 간단하고 빠름**) Git repo에서 민감한 자료를 삭제할 수 있는 프로그램
    - The `git filter-repo`tool and the BFG Repo-Cleaner rewrite your repository's history, which changes the SHAs for existing commits that you alter and any dependent commits. Changed commit SHAs may affect open pull requests in your repository.
    - java가 설치되어 있어야 함
    
    - 방법
        1. bfg.jar 파일 다운로드 ⇒ [BFG Repo-Cleaner by rtyley](https://rtyley.github.io/bfg-repo-cleaner/)
        
        1. 바꾸고자 하는 repo를 **mirror clone**
            
            This is a [bare](https://git-scm.com/docs/gitglossary.html#def_bare_repository) repo, which means **your normal files won't be visible**, but it is a ***full*
             copy** of the Git database of your repository, and at this point you should **make a backup of it** to ensure you don't lose anything.
            
            ```bash
            $ git clone --mirror git://example.com/some-big-repo.git
            ```
            
        
        1. 다운받은 bfg.jar파일을 실행
            
            ```bash
            $ java -jar bfg.jar --strip-blobs-bigger-than 100M some-big-repo.git
            ```
            
        
        1. 
            
            4.1 삭제하고자 하는 파일 입력
            
            ```bash
            $ bfg --delete-files YOUR-FILE-WITH-SENSITIVE-DATA
            ```
            
            4.2 txt를 통해 삭제하고 싶은 텍스트도 삭제 가능(txt파일 통해)
            
            ```bash
            $ bfg --replace-text passwords.txt
            ```
            
        
        1. clone한 shell push 해 주면 된다(cloned repository directory에서 실행)
            
            ```bash
            $ git push
            
            // 작동을 안 한다면 이걸로
            $ git push --force
            ```
            

참고사이트

[Removing sensitive data from a repository - GitHub Docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository#fully-removing-the-data-from-github)

[BFG Repo-Cleaner by rtyley](https://rtyley.github.io/bfg-repo-cleaner/)