# Today I Define ( TID ) π§  

### β¨μΆμμλ£β¨ 
  * [![29](https://user-images.githubusercontent.com/53691249/153768166-1c7d7c43-0405-441e-8381-32af0273b4c4.png)](https://apps.apple.com/kr/app/%ED%8B%B0%EB%93%9C-%EB%82%98%EB%8A%94-%EC%9D%B4%EA%B1%B8-%EC%9D%B4%EB%A0%87%EA%B2%8C-%EB%B6%80%EB%A5%B4%EA%B8%B0%EB%A1%9C-%ED%96%88%EB%8B%A4/id1597847159)
  * [κ°λ° μ΄ν°λ μ΄μ : κ³΅μμ°μ , κ°λ°μ΄μ μ λ¦¬](https://jasper-atom-7c6.notion.site/9becfca153ff4e00a180a0e58228ef5c)
  * [μλ°μ΄νΈ μ΄ν°λ μ΄μ : κ³΅μμ°μ ](https://jasper-atom-7c6.notion.site/be2d3b61f3af42f48d850b9efc69dc8c)


### μ£Όμ κΈ°μ  μ€ν
`Realm` `AutoLayout` `Snapkit` `Alamofire` `SwiftyJSON` `MVC` `Github Action`

###  β νμ΅ λ° μ μ© ππ»ββοΈ

### 1. AutoLayout 
  * μ½ 1μ£ΌμΌ ~ 2μ£ΌμΌ λμ κΎΈμ€ν λ μ΄μμμ μ‘°μ ν΄λ΄€μ΅λλ€. μμκ°μ΄ μλ λΉμ¨μ κΈ°μ€μΌλ‘ μ‘°μ νλ κ²μ΄ μκ°μ μΈ μΈ‘λ©΄, κ°λ° μλ μΈ‘λ©΄μμ λ ν¨μ¨μ΄ μ’μκ³  νλ°λΆ μμμμλ λλΆλΆ λΉμ¨μ μ΄μ©ν΄ λ μ΄μμμ λ§λ€μμ΅λλ€.
 
  <details>
<summary>λ μ΄μμ νΌμ³λ³΄κΈ°</summary>
<div markdown="1">
 <br></br>

  ![αα³αα³αα΅α«αα£αΊ 2022-02-14 αα©αα₯α« 4 07 40](https://user-images.githubusercontent.com/53691249/153770662-83d5642a-b010-4039-b0c6-65f754789b59.png)
 
</div>
</details>

### 2. Realm 
  * μ μ κ° μ μ₯ν λ¨μ΄λ₯Ό κΈ°λ‘νκΈ° μν΄ Realmμ μ ννμ΅λλ€.
  * κ°λ°μ΄μ: κΈ°μ‘΄μ μ€ν€λ§μμ λ μ§ λ°μ΄ν°λ₯Ό μ μ₯ν  μμ μμλ‘ λ£μ΄λλ Date()κ°μ μλ‘μ΄ Objectλ‘ λ³νν΄μΌνμ΅λλ€. Realmμ μ€ν€λ§λ₯Ό λ³κ²½ν΄μΌ λλ€λ κ² μ¦, **λ§μ΄κ·Έλ μ΄μ**μ ν  μ μλ€λ κ²μ μμκ³  λ€μκ³Ό κ°μ΄ μ μ©ν¨μΌλ‘μ¨ λ¬Έμ λ₯Ό ν΄κ²°ν  μ μμμ΅λλ€.

     ```swift
     //Realm migration
     let config = Realm.Configuration(
         schemaVersion: 2,
         migrationBlock: { migration,oldSchemaVersion in
             if (oldSchemaVersion < 2){
                 migration.enumerateObjects(ofType: DefineWordModel.className()) { oldObject, newObject in
                     //κΈ°μ‘΄μ λ μ§λ€μ λ³ννκ³  storedDateμ κ°μ λ¨κΈ΄λ€.
                     let format = DateFormatter()
                     format.dateFormat = "yyyyλ MMμ ddμΌ"
                     let value = format.string(from: oldObject?["date"] as! Date)

                     newObject?["storedDate"] = value
                 }
             }

         }
     )
     ```
     
### 3. Alamofire + SwiftyJson
  * API ν΅μ μ μν΄ Alamofireλ₯Ό, λ°μμ¨ Json ννμ λ°μ΄ν°λ₯Ό μ¬μ©νκΈ° μν΄ SwiftyJSONμ νμ΅ λ° μ μ©νμ¬ λλ€μΌλ‘ λμ¨ λ¨μ΄μ μ μλ₯Ό λ³΄μ¬μ€ μ μμμ΅λλ€.

### 4. Network Status
  * μΆμ² λ¨μ΄ κΈ°λ₯μ λ§λ€κΈ° μν΄ [μ°λ¦¬λ§μAPI](https://opendict.korean.go.kr/service/openApiInfo)λ₯Ό μ΄μ©νμ΅λλ€. 
  * μ΄κΈ°, μ μ μ λ€νΈμν¬ μνλ₯Ό κ³ λ €νμ§ μκ³  μ½λλ₯Ό μμ±νκ³  μ΄λ₯Ό λ°κ²¬νκ³  μνλ ViewControllerμ μ§μμ Network μνλ₯Ό νμΈν  μ μκ² ν¨μλ₯Ό λ§λ€μ΄ μ¬μ©νμ΅λλ€.

    ```swift
     import Network
    //λ€νΈμν¬ μν λͺ¨λν°
    func monitorNetwork(){

        let monitor = NWPathMonitor()

        monitor.pathUpdateHandler = {
            path in
            if path.status == .satisfied {
                DispatchQueue.main.async {
                    return
                }
            } else {
                DispatchQueue.main.async {

                    self.showAlert(title: "λ€νΈμν¬μ μ°κ²°λμ΄ μμ§ μμμ.\nμ€μ νλ©΄μΌλ‘ μ΄λν©λλ€ π₯²",connection: true)
                }
            }
        }
        let queue = DispatchQueue(label: "Network")
        monitor.start(queue: queue)
    }
    ```
    
### 5. μ νλ¦¬μΌμ΄μ μΆμ κ³Όμ  
  * μλΉμ€λ₯Ό κ°λ°νκ³  λ°°ν¬νλ μΌλ ¨μ κ³Όμ μ κ²½νν  μ μμμ΅λλ€.
  * μλ°μ΄νΈλ₯Ό κΎΈμ€ν μ§ννκ³  μμ΅λλ€. ( κ°μ₯ μ΅κ·Ό μλ°μ΄νΈ : 2022.3.14 ) 
  
### 6. CI / CD νμ΅ λ° μ μ©
  <details>
 <summary> νμ΅ λ΄μ© μ λ¦¬ </summary>
 <div markdown="1">
  
  - CI / CD Github Action
    - Github Actionμ΄λ 
        - Pull Request, Push λ±μ μ΄λ²€νΈ λ°μμ λ°λΌ μλνλ μμμ μ§νν  μ μκ² ν΄μ£Όλ κΈ°λ₯
        - CI / CD
            - λ‘μ»¬ λ ν¬μ§ν λ¦¬μμ μκ²© λ ν¬μ§ν λ¦¬λ‘ νΈμ¬νκ³  λ ν, Github Actionsμμλ μ΄λ²€νΈ λ°μμ λ°λΌ μλμΌλ‘ λΉλ λ° λ°°ν¬νλ μ€ν¬λ¦½νΈλ₯Ό μ€νμμΌμ£Όλ κ²
        - Testing
            - Pull Requestλ₯Ό λ³΄λ΄λ©΄ μλμΌλ‘ νμ€νΈλ₯Ό μ§ννλ κ² λν κ΅¬ν κ°λ₯νκ³  μλμΌλ‘ Pull Requestλ₯Ό open λ° closeν  μ μκ² λ¨
        - Cron Job
            - νΉμ ν μκ°λμ μ€ν¬λ¦½νΈλ₯Ό λ°λ³΅ μ€νν  μ μμ
    - Github Actionμ κ΅¬μ±μμ
        - Workflow
            - λ ν¬μ μΆκ°ν  μ μλ μλν μ»€λ§¨λμ μ§ν©μΌλ‘ νλ μ΄μμ JobμΌλ‘ κ΅¬μ±λμ΄ μμΌλ©° Pushλ PRκ³Ό κ°μ μ΄λ²€νΈμ μν΄ μ€νλ  μλ μμΌλ©° νΉμ  μκ°λμ μ€νλ  μλ μμ
                
               ![Untitled](https://user-images.githubusercontent.com/53691249/169544406-155d6cee-4ccb-4350-a876-d9599202c006.png)
                
        - Event
            - Workflowλ₯Ό μ€νμν€λ νΉμ  νλ ( Push, Pull Request, Commit λ± )μ μλ―Έ ν¨
        - Job
            - Jobμ΄λ λμΌν Runnerμμ μ§νλλ Stepμ μ§ν©
            - νλμ workflow λ΄μ μ¬λ¬ Jobμ λλ¦½μ μΌλ‘ μ€νλλ νμμ λ°λΌ μμ‘΄ κ΄κ³λ₯Ό μ€μ νμ¬ μμλ₯Ό μ§μ ν  μ μμ
                - κ°λ Ή Test μμκ³Ό Build μμμ μννλ Jobλ€μ΄ νλμ workflowμ μ‘΄μ¬νλ€λ©΄ Build μ΄νμ Testκ° μ§νλμ΄μΌ νκΈ° λλ¬Έμ Build Jobμ΄ λ§λ¬΄λ¦¬ λ ν Test Jobμ μ€νν  μ μλλ‘ μ§μ κ°λ₯ ( Build μ€ν¨μ Testλ μ€ννμ§ μμ )
        - Step
            - μ»€λ§¨λλ₯Ό μ€νν  μ μλ κ°κ°μ Taskλ₯Ό μλ―Ένκ³ , Shell μ»€λ§¨λκ° λ  μλ μκ³ , νλμ Actionμ΄ λ  μλ μμ
            - νλμ Job λ΄μμ κ°κ°μ Stepμ λ€μν Taskλ‘ μΈν΄ μμ±λ λ°μ΄ν°λ₯Ό κ³΅μ ν  μ μμ
        - Action
            - Jobμ λ§λ€κΈ° μν΄ Stepμ κ²°ν©ν μ»€λ§¨λλ‘ μ¬μ¬μ©μ΄ κ°λ₯ν Workflowμ κ°μ₯ μμ λ¨μ
            - μ§μ  λ§λ€κ±°λ Github Communityμ μν΄ μμ±λ Actionμ λΆλ¬μ μ¬μ©ν  μ μμ
        - Runner
            - Runnerλ Github Actions Workflow λ΄μ μλ Jobμ μ€νμν€κΈ° μν μ νλ¦¬μΌμ΄μ
            - Runner Applicationμ Githubμμ νΈμ€ννλ κ°μνκ²½ νΉμ μ§μ  νΈμ€ννλ κ°μ νκ²½μμ μ€ν κ°λ₯νλ©° Githubμμ νΈμ€ννλ κ°μ μΈμ€ν΄μ€μ κ²½μ° λ©λͺ¨λ¦¬ λ° μ©λ μ νμ΄ μ‘΄μ¬
        
    - Workflow μμ± λ° νμΌ μ€λͺ
        - .github/workflows λλ ν λ¦¬ λ΄μ .yml νμΌμ μμ±ν΄λ λμ§λ§, Repositoryμ Actions ν­μμ μλμΌλ‘ templateλ₯Ό λ§λ€μ΄μ£Όλ κΈ°λ₯μ μ¬μ©νλ κ²μ΄ μ’μ
        - Githubμμ μ κ³΅νλ κ°μ₯ κΈ°λ³Έμ μΈ Templateλ set up a workflow yourselfλ₯Ό ν΄λ¦­
            
            ![αα³αα³αα΅α«αα£αΊ 2022-05-20 αα©αα? 9 56 20](https://user-images.githubusercontent.com/53691249/169544548-920fb460-2134-4b8a-b80a-92e5a9c43795.png)
            
        
        - λ€μκ³Ό κ°μ μμμ .yml νμΌμ΄ μμ±λ¨
            
            ![αα³αα³αα΅α«αα£αΊ 2022-05-20 αα©αα? 9 58 50](https://user-images.githubusercontent.com/53691249/169544604-da39ac8f-b665-4b2c-a8d0-f62a880e7b60.png)
            
        - μ€λͺ
            
            ```yaml
            # Actions ν­μ νμλ  Workflow μ΄λ¦
            name: CI
            
            # Workflowλ₯Ό μ€νμν€κΈ° μν Event λͺ©λ‘
            on: # νΈλ¦¬κ±°
              # νλ¨ μ½λμ λ°λΌ develop λΈλμΉμ Push λλ Pull Request μ΄λ²€νΈκ° λ°μν κ²½μ°μ Workflowκ° μ€ν
              push:
                branches: [main]
              # νΉμ ν Branchμ νΈμ¬λμμ λ μ¬μ©νλ €λ©΄ κ°λ Ή feature/*λ‘ μμ±νλ©΄ λ¨
              pull_request:
                branches: [main]
            
              # ν΄λΉ μ΅μμ ν΅ν΄ Actions ν­μμ Workflowλ₯Ό μ€ν
              workflow_dispatch:
            
            # Workflowμ νλ μ΄μμ Job 
            jobs:
              # Job μ΄λ¦μΌλ‘, buildλΌλ μ΄λ¦μΌλ‘ Jobμ΄ νμ
              build:
                # Runnerκ° μ€νλλ νκ²½μ μ μ
                runs-on: macos-latest
            
                # build Job λ΄μ step λͺ©λ‘
                steps:
                  # uses ν€μλλ₯Ό ν΅ν΄ Actionμ λΆλ¬μ΄
                  # μ¬κΈ°μμλ ν΄λΉ λ ν¬μ§ν λ¦¬λ‘ check-out λ° λ ν¬μ§ν λ¦¬μ μ κ·Όν  μ μλ Actionμ λΆλ¬μ΄.
                  - uses: actions/checkout@v2
                  # μ€νλλ μ»€λ§¨λμ λν μ€λͺμΌλ‘, Workflowμ νμ
                  - name: Build
                    run: echo Hello, world!
            
                  # νλμ μ»€λ§¨λκ° μλ μ¬λ¬ μ»€λ§¨λλ μ€ν κ°λ₯
                  - name: Run tests
                    run: |
                      xcodebuild test -project "$XC_PROJECT" -scheme "$XC_SCHEME" -destination 'platform=iOS Simulator,name=iPhone 13'
            ```
  
       - Start Commit ν Action ν­μ νμΈν΄λ³΄λ©΄ λ€μκ³Ό κ°μ΄ μ μμ μΌλ‘ μλν κ²μ νμΈν  μ μμ.
        ![αα³αα³αα΅α«αα£αΊ 2022-05-23 αα©αα? 10 41 42](https://user-images.githubusercontent.com/53691249/169832694-e4414be0-3ec1-4054-9cac-bd174721ffb6.png)


 </div>
 </details>
 
  <details>
 <summary> Github Action λ°©λ² λ° μ μ©</summary>
 <div markdown="2">
  
  - μ°Έκ³ : [naljinλμ λΈλ‘κ·Έ](https://sujinnaljin.medium.com/ci-cd-github-actions-%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-testflight-%EC%97%85%EB%A1%9C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-8ecdbeb227a3)
  - μ μ©λ°©λ² 
  
    ```
    name: TID Automation release
    
    on:
      push:
        branches: [ main ]
      pull_request:
        branches: [ main ]
    
    jobs:
      build:
      
        runs-on: macos-latest
        env: 
    			   # κ°μνκ²½
    		   	# Xcode λ²μ  λ° νλ‘μ νΈμ μ€ν€λ§ μ€μ  + μ¬μ©ν  ν€μ²΄μΈ μ€μ  ( μ€ν¬λ¦½νΈμμ λ§λ€μ΄μ λ£μ λ³μ )
          XC_VERSION: ${{ '13.1' }}
          XC_PROJECT: ${{ 'usket_TID.xcodeproj' }}
          XC_SCHEME: ${{ 'usket_TID' }}
          KEYCHAIN: ${{ 'usket.keychain' }}
          # λ£¨νΈ
          PROJECT_ROOT_PATH: ${{ 'usket_TID' }}
           
          ENCRYPTED_CERTS_FILE_PATH: ${{ '.github/secrets/GithubActionKey.p12.gpg' }}
    			   # μ΄λμ λ³΅νΈν ν  κ²μΈμ§ λͺμ
          DECRYPTED_CERTS_FILE_PATH: ${{ '.github/secrets/GithubActionKey.p12' }}
    
          ENCRYPTED_PROVISION_FILE_PATH: ${{ '.github/secrets/GithubAction.mobileprovision.gpg' }}
    		   	# μ΄λμ λ³΅νΈν ν  κ²μΈμ§ λͺμ
          DECRYPTED_PROVISION_FILE_PATH: ${{ '.github/secrets/GithubAction.mobileprovision' }} 
    			
    			   # κΈ°μ‘΄μ secretsλ₯Ό κ°μ§κ³ μμ μ μ©
          CERTS_EXPORT_PWD: ${{ secrets.CERTS_EXPORT_PWD }}
          CERTS_ENCRYPTION_PWD: ${{ secrets.CERTS_ENCRYPTO_PWD }}
          PROFILES_ENCRYPTO_PWD: ${{ secrets.PROFILES_ENCRYPTO_PWD }}
    			
    			   # μμΉ΄μ΄λΈ path λ° μ±μ€ν μ΄μ μ¬λ¦΄ artifacts path μ€μ  
          XC_ARCHIVE_PATH: ${{ 'usket_TID.xcarchive' }}
          XC_EXPORT_PATH: ${{ './artifacts' }}
          
        steps:
          - name: Select Xcode Version
            run: "sudo xcode-select -s /Applications/Xcode_$XC_VERSION.app"
        
          - uses: actions/checkout@v3
    
          - name: Build
            run: echo Hello, world!
    			
    			   # μμμ λ§λ€μ΄λ ν€μ²΄μΈ μ μ©
          - name: Configure Keychain 
            run: | 
              security create-keychain -p "" "$KEYCHAIN" 
              security list-keychains -s "$KEYCHAIN" 
              security default-keychain -s "$KEYCHAIN" 
              security unlock-keychain -p "" "$KEYCHAIN"
              
    			   # Code Signing μ€ν
          - name : Configure Code Signing
            run: | 
              gpg -d -o "$DECRYPTED_CERTS_FILE_PATH" --pinentry-mode=loopback --passphrase "$CERTS_ENCRYPTION_PWD" "$ENCRYPTED_CERTS_FILE_PATH"
              gpg -d -o "$DECRYPTED_PROVISION_FILE_PATH" --pinentry-mode=loopback --passphrase "$PROFILES_ENCRYPTO_PWD" "$ENCRYPTED_PROVISION_FILE_PATH"
              security import "$DECRYPTED_CERTS_FILE_PATH" -k "$KEYCHAIN" -P "$CERTS_EXPORT_PWD" -A
              security set-key-partition-list -S apple-tool:,apple: -s -k "" "$KEYCHAIN"
              mkdir -p "$HOME/Library/MobileDevice/Provisioning Profiles"
              echo `ls .github/secrets/*.mobileprovision`
              # νλ‘νμΌλ€μ renameνκ³  μλ‘λ§λ  λλ ν λ¦¬μ λ³΅μ¬
              for PROVISION in `ls .github/secrets/*.mobileprovision`
                do
                  UUID=`/usr/libexec/PlistBuddy -c 'Print :UUID' /dev/stdin <<< $(security cms -D -i ./$PROVISION)`
                cp "./$PROVISION" "$HOME/Library/MobileDevice/Provisioning Profiles/$UUID.mobileprovision"
                done
    			   # μμΉ΄μ΄λΈ!
          - name: Archive
            working-directory: usket_TID
            run: | 
              mkdir artifacts
              xcodebuild archive -project "$XC_PROJECT" -scheme "$XC_SCHEME" -configuration release -archivePath "$XC_ARCHIVE_PATH"
    			   # App Storeλ‘ λ΄λ³΄λ΄κΈ°
          - name: Export for App Store
            run: | 
              xcodebuild -exportArchive -archivePath "$XC_ARCHIVE_PATH" -exportOptionsPlist ExportOptions.plist -exportPath "$XC_EXPORT_PATH"
    
    			   # μλ‘λνλ©΄ λ!
          - name: Upload Artifact
            uses: actions/upload-artifact@v3
            with:
              name: Artifacts
              path: ./artifacts
    ```
  - Artifactκ° μλ TestFlightλ‘ μ μ©λ°©λ²
    - κ²½λ‘λ₯Ό μ°Ύμ μ μλ€λ μ€λ₯ μ€λ₯ 
      - apple-actions/upload-testflight-build@v1λ₯Ό μ΄μ©νμ κ²½μ° λ°μν¨. 
      - λλ ν λ¦¬μ νμΌμ μμ±νλ λ°©λ²μΌλ‘ λ³κ²½ 
    
    - μλ μ μ© νμ΄μΌνλ λ°©λ²
                                                                                         
      ```
       name: Upload app to TestFlight 
       uses: apple-actions/upload-testflight-build@v1
        with:
            app-path: 'usket_TID.ipa'
            issuer-id: ${{ secrets.APPSTORE_ISSUER_ID }}
            api-key-id: ${{ secrets.APPSTORE_API_KEY_ID }}
            api-private-key: ${{ secrets.APPSTORE_API_PRIVATE_KEY }}
      ```
                                                                                         
    - λ€μκ³Ό κ°μ΄ λλ ν λ¦¬λ₯Ό μμ± λ° base64λ‘ μΈμ½λ©λ Private Keyλ₯Ό decodingνμ¬ λ£μ ν μ€ν κ·Έλ¦¬κ³  μ±κ³΅!
        
        ```
        name: TID Automation release
        
        on:
          push:
            branches: [ main ]
          pull_request:
            branches: [ main ]
        
        jobs:
          build:
            runs-on: macos-latest
            env: # κ°μνκ²½
              XC_VERSION: ${{ '13.1' }}
              XC_PROJECT: ${{ 'usket_TID.xcodeproj' }}
              XC_SCHEME: ${{ 'usket_TID' }}
              XC_ARCHIVE_PATH: ${{ 'usket_TID.xcarchive' }}
              KEYCHAIN: ${{ 'usket.keychain' }}
              
              ENCRYPTED_CERTS_FILE_PATH: ${{ '.github/secrets/GithubActionKey.p12.gpg' }}
              DECRYPTED_CERTS_FILE_PATH: ${{ '.github/secrets/GithubActionKey.p12' }} # μ΄λμ λ³΅νΈν ν  κ²μΈμ§ λͺμ
        
              ENCRYPTED_PROVISION_FILE_PATH: ${{ '.github/secrets/GithubAction.mobileprovision.gpg' }}
              DECRYPTED_PROVISION_FILE_PATH: ${{ '.github/secrets/GithubAction.mobileprovision' }} # μ΄λμ λ³΅νΈν ν  κ²μΈμ§ λͺμ
        
              CERTS_EXPORT_PWD: ${{ secrets.CERTS_EXPORT_PWD }}
              CERTS_ENCRYPTION_PWD: ${{ secrets.CERTS_ENCRYPTO_PWD }}
              PROFILES_ENCRYPTO_PWD: ${{ secrets.PROFILES_ENCRYPTO_PWD }}
        
            steps:
              - name: Setting checkout
                uses: actions/checkout@v3
                  
              - name: Select Xcode Version
                run: "sudo xcode-select -s /Applications/Xcode_$XC_VERSION.app"
        
              - name: Configure Keychain 
                run: | 
                  security create-keychain -p "" "$KEYCHAIN" 
                  security list-keychains -s "$KEYCHAIN" 
                  security default-keychain -s "$KEYCHAIN" 
                  security unlock-keychain -p "" "$KEYCHAIN"
                  security set-keychain-settings -lut 1200
                  security list-keychains
                  
              - name : Configure Code Signing
                run: | 
                  gpg -d -o "$DECRYPTED_CERTS_FILE_PATH" --pinentry-mode=loopback --passphrase "$CERTS_ENCRYPTION_PWD" "$ENCRYPTED_CERTS_FILE_PATH"
                  gpg -d -o "$DECRYPTED_PROVISION_FILE_PATH" --pinentry-mode=loopback --passphrase "$PROFILES_ENCRYPTO_PWD" "$ENCRYPTED_PROVISION_FILE_PATH"
                  security import "$DECRYPTED_CERTS_FILE_PATH" -k "$KEYCHAIN" -P "$CERTS_EXPORT_PWD" -A
                  security set-key-partition-list -S apple-tool:,apple: -s -k "" "$KEYCHAIN"
                  mkdir -p "$HOME/Library/MobileDevice/Provisioning Profiles"
                  echo `ls .github/secrets/*.mobileprovision`
                  # νλ‘νμΌλ€μ renameνκ³  μλ‘λ§λ  λλ ν λ¦¬μ λ³΅μ¬
                  for PROVISION in `ls .github/secrets/*.mobileprovision`
                    do
                      UUID=`/usr/libexec/PlistBuddy -c 'Print :UUID' /dev/stdin <<< $(security cms -D -i ./$PROVISION)`
                    cp "./$PROVISION" "$HOME/Library/MobileDevice/Provisioning Profiles/$UUID.mobileprovision"
                    done
              - name: Archive App
                working-directory: usket_TID
                run: | 
                  xcodebuild clean archive -project "$XC_PROJECT" -scheme "$XC_SCHEME" -configuration release -archivePath "$XC_ARCHIVE_PATH" 
                  
              - name: Export App
                working-directory: usket_TID
                run:  |
                  xcodebuild -exportArchive -archivePath "$XC_ARCHIVE_PATH" -exportOptionsPlist ExportOptions.plist -exportPath . -allowProvisioningUpdates
                  ls
        			# Make Private API Key Path
              - name: Install private API key P8
                env:
                  APPSTORE_API_PRIVATE_KEY: ${{ secrets.APPSTORE_API_PRIVATE_KEY }}
                  APPSTORE_API_KEY_ID: ${{ secrets.APPSTORE_API_KEY_ID }}
        				# Decode Private Key
                run: | 
                  mkdir -p ~/private_keys
                  echo -n "$APPSTORE_API_PRIVATE_KEY" | base64 --decode --output ~/private_keys/AuthKey_$APPSTORE_API_KEY_ID.p8
                  ls
                  
              - name: Upload app to TestFlight
                env:
                  APPSTORE_API_KEY_ID: ${{ secrets.APPSTORE_API_KEY_ID }}
                run: |
                  cd usket_TID
                  ls
                  xcrun altool --output-format xml --upload-app -f usket_TID.ipa -t ios --apiKey $APPSTORE_API_KEY_ID --apiIssuer ${{ secrets.APPSTORE_ISSUER_ID }}
        ```
                                                                
   </div>
 </details>
 
### 7. μλ°μ΄νΈ

  |λ²μ |μλ°μ΄νΈ λ΄μ­|μ€λͺ|
  |:---:|:---:|:---:|
  |v2.11|μλ¦Ό μ€μ μ μλ¦Όμ΄ μ€μ§ μλ μ€λ₯| TimeZone μ΄μλ‘ μΈν μλ¦Όμ΄ μ΄μ  λ μ§μ λ±λ‘λλ μ΄μλ₯Ό ν΄κ²°νμ΅λλ€.|
  |v2.10|Calendarμμ λ¨μ΄ ν΄λ¦­ μ΄λ²€νΈ|μμ  νμ΄μ§λ‘ μ΄λκ°λ₯νκ² λ³κ²½νμ΅λλ€.|
  |v2.08|CI / CD <br /> [ [CI/CD κΈ°λ‘(1) - λΈλ‘κ·Έ](https://pooh-footprints.tistory.com/66) ] <br /> [ [CI/CD κΈ°λ‘(2) - λΈλ‘κ·Έ](https://pooh-footprints.tistory.com/67) ] <br /> [ [CI/CD κΈ°λ‘(3) - λΈλ‘κ·Έ](https://pooh-footprints.tistory.com/68) ]|Github Actionμ μ΄μ©ν΄ μλλ°°ν¬ νκ²½μ κ΅¬μΆνμ΅λλ€.|
  |v2.07|μΆκ°λ²νΌ ν΄λ¦­μ Navigation Barλ Ove lay Viewμ κ°μΈμ§μ§ μλ μ€λ₯ <br /> [ [νΈλ¬λΈ μν κΈ°λ‘ - λΈλ‘κ·Έ](https://pooh-footprints.tistory.com/65) ]|ν΄λΉ μ€λ₯λ₯Ό ν΄κ²°νμ΅λλ€.| 
  |v2.06|κ°λ°, μ±, λ¦¬λ·°λ₯Ό μν λ²νΌ μμ±|κ°κ° κΉνλΈ λ§ν¬, μ±κ΄λ ¨ λ¬Έμλ₯Ό μν μ΄λ©μΌ λ₯λ§ν¬, λ¦¬λ·°νμ΄μ§λ‘ μ΄λκ°λ₯ν λ²νΌμ μΆκ°|
  |v2.05|λ‘κ³  λμμ΄λ μΈμ€νκ·Έλ¨ λ§ν¬|μμ²­μΌλ‘ λ§ν¬λ₯Ό μ²¨λΆ λ° μΆκ° λ²νΌ ν΄λ¦­μ λ°±κ·ΈλΌμ΄λ μ»¬λ¬ |
  |v2.04|Firebase Crashlytics|Firebase Crashlytics μ μ© λ° κΈ°ν μ€λ₯μμ |
  |v2.03|Local Notification <br /> [ [νΈλ¬λΈ μν κΈ°λ‘ - λΈλ‘κ·Έ](https://pooh-footprints.tistory.com/58) ]|λ‘μ»¬μλ¦Όμ ν λ¨μ΄λ§ λ°λ³΅ν΄μ μ€λ μ΄μλ₯Ό μμ νμ΅λλ€.|
  |v2.02|λ μ΄μμ λ²κ·Έ μμ |λ¨μ΄μ λ»μ λ³΄μ¬μ£Όλ TableViewμμ λ°μν μ€λ₯λ₯Ό ν΄κ²°νμ΅λλ€.|
  |v2.01|ν΅κ³ λ²κ·Έ μμ |μμ΄ λ°λκ² λ  κ²½μ° κΈ°μ‘΄μ μ½λμμ nil μ²λ¦¬λ‘ μΈν μ€λ₯κ° μ‘΄μ¬, μ΄λ₯Ό ν΄κ²°νμ΅λλ€.|
  |v2.0|λ μ΄μμ λ²κ·Έ μμ , μ μ₯μ ν μ€νΈ μλ¦Ό μΆκ°, μμ±νμ΄μ§ μ§μμ μ μ€μ²λ‘ λκ°κΈ° μΆκ°, μμ μ κ°μ κ³Ό μ μ λΆλΆκΉμ§ λ³΄μ΄κ² μΆκ°, ν΅κ³μμΉ μμ , λ‘μ»¬ μλ¦Ό μ κ³΅, μΊλ¦°λ μ κ³΅|μΆμ μ΄ν λ°μ νΌλλ°± ν΅ν΄ μΊλ¦°λμ μλ¦Όμ μ€μ μΌλ‘ μλ°μ΄νΈλ₯Ό μ§ννμ΅λλ€.|
  
  ---
  
  
###  β λ°°μμΌν  & λ°°μ°κ³  μΆμ κΈ°μ  ππ»ββ
  * SnapKit
    * λ μ΄μμμ κ΄ν΄ κ²μμ νλ€λ³΄λ©΄ SnapKitμ λν λ΅λ³λ€μ΄ λ§μ΄ λ³΄μμ΅λλ€. μ½λλ₯Ό μ΄μ©νμ¬ λ μ΄μμμ μ‘μ μ μλ λΌμ΄λΈλ¬λ¦¬μΈ κ²μ μκ² λμκ³  κ°λ¨ν λ·°λ€ κ°μ κ²½μ°μλ μ½λλ₯Ό νμ©ν΄ λ§λ λ€λ©΄ κ°λ°μκ° μΈ‘λ©΄μμ λμ νμ©λκ° μλ€κ³  μκ°μ΄ λ€μκ³  λ€μ νλ‘μ νΈμλ μ΄λ₯Ό μ κ·Ήμ μΌλ‘ νμ©ν΄ λ³΄κ³  μΆμ΄μ‘μ΅λλ€.

  * MVVM
    * κΈ°μ‘΄μ MVC λμμΈ ν¨ν΄μ νμ©νμ¬ κ°λ°μ νμ§λ§ ViewControllerμ UI + κΈ°λ₯μ λͺ¨λ λ£μ΄μΌνκΈ°μ κ°λμ±μ΄ λ¨μ΄μ§λ€κ³  μκ°μ΄ λ€μμ΅λλ€. κ°λμ±μ λμ΄κ³  ν¨μ¨μ μΈ κ΄λ¦¬λ₯Ό μν΄ MVVM λμμΈ ν¨ν΄μ νμ΅ν΄μΌ κ² λ€κ³  λ€μ§νμ΅λλ€. 
<br></br>
