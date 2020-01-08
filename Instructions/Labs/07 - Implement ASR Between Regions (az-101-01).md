---
lab:
    title: 'Azure 지역 간에 Site Resovert 자격 증명 구현'
    module: '모듈 07 - 데이터 보호'
---

# 랩: Azure 지역 간에 Site Resovert 자격 증명 구현

이 랩의 모든 작업은 Azure 포털에서 수행됩니다

랩 파일: 

-  **Labfiles\\Module_07\\Azure_Site_Recovery_Between_Regions\\az-101-01_azuredeploy.json**

-  **Labfiles\\Module_07\\Azure_Site_Recovery_Between_Regions\\az-101-01_azuredeploy.parameters.json**

### 시나리오
  
Adatum Corporation은 Site Resovert 자격 증명를 구현하여 지역간 Azure VM 마이그레이션 및 보호를 구성합니다.


### 목표
  
이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다:

-  Azure Site Resovert 자격 증명 구현

-  Azure Site Resovert 자격 증명을 사용하여 Azure 영역 간의 Azure VM 복제 구성


### 연습 1: Site Resovert 자격 증명를 사용하여 Azure VM 마이그레이션을 위한 사전 요구 사항 구현 
  
이 연습의 주요 작업은 다음과 같습니다:

1. Azure Resource Manager 템플릿을 사용하여 마이그레이션 할 Azure VM 배포

1. Azure 복구 서비스 자격 증명 모음 만들기
  

#### 작업 1: Azure Resource Manager 템플릿을 사용하여 마이그레이션 할 Azure VM 배포

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com) 에서 Azure 포털을 찾아보고 이 랩에서 사용하려는 Azure 구독에서 소유자 역할이 있는 Microsoft 계정을 사용하여 로그인합니다.

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Template deployment**를 검색합니다. 그리고 **Template deployment (deploy using custom templates)**를 선택합니다.

1. **만들기**를 클릭합니다.
 
1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 를 선택합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일인 **Labfiles\\Module_07\\Azure_Site_Recovery_Between_Regions\\az-101-01_azuredeploy.json** 을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Windows Server 2016 데이터 센터를 호스팅하는 Azure VM의 배포를 정의합니다.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\Module_07\\Azure_Site_Recovery_Between_Regions\\az-101-01_azuredeploy.parameters.json**을 로드합니다.

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음을 사용하여 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1010101-RG** 이름으로 새로 만들기

    - 지역: 랩 지역에 가장 가까운 Azure 지역의 이름 및 Azure VM을 프로비전할 수 있는 지역

    - Vm Name: **az1010101-vm**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Image Publisher: **MicrosoftWindowsServer**

    - Image Offer: **WindowsServer**

    - Image SKU: **2016-Datacenter-Server-Core-smalldisk**

    - Vm Size: **Standard_DS1_v2**

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 식별하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)참고하십시오.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 작업으로 진행합니다. 이 실습의 두 번째 실습에서 가상 시스템 **az1010101-vm** 을 사용합니다.


#### 작업 2: Site Recovery 자격 증명 모음 구현
 
1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Backup and Site Recovery** 를 검색합니다.

1. 검색 결과 목록을 사용하여 **Recovery Services 자격 증명 모음 만들기** 블레이드로 이동합니다.

1. **Recovery Services 자격 증명 모음 만들기** 블레이드를 사용하여 다음 설정을 사용하여 Site Recovery 자격 증명을 만듭니다:

    - 구독: 이 연습의 이전 작업에서 사용한 것과 동일한 Azure 구독

    - 리소스 그룹: **az1010102-RG** 이름으로 새로 만들기

    - 자격 증명 모음 이름: **vaultaz1010102**

    - 지역: 이 연습의 이전 작업에서 Azure VM을 배포한 지역과 다른 구독에서 사용할 수 있는 Azure 영역의 이름입니다

    > **참고**: 배포가 완료될 때 까지 기다립니다. 약 1분 정소 소요됩니다.

1. Azure Portal에서 새로 배포된 Site Recovery 자격 증명 모음인 **vaultaz1010102** 블레이드로 이동합니다.

1. **vaultaz1010102** 블레이드에서 **속성** 블레이드로 이동한 다음 **보안 설정**에 있는 **업데이트**를 클릭합니다.

1. **보안 설정** 블레이드가 뜨면 **일시 삭제 (Azure Virtual Machines용)**를 **사용 안 함**으로 수정하고 저장합니다.

> **결과**: 이 연습을 완료한 후 Azure Resource Manager 템플릿을 사용하여 Azure VM 배포를 시작하고 Azure VM 디스크 파일의 콘텐츠를 복제하는 데 사용할 Site Resovert 자격 증명 모음을 만들었습니다. 


### 연습 2: Site Resovert 자격 증명를 사용하여 Azure 지역 간에 Azure VM 마이그레이션 

이 연습의 주요 작업은 다음과 같습니다:

1. Azure VM 복제 구성

1. Azure VM 복제 설정 검토 


#### 작업 1: Azure VM 복제 구성

   > **참고**: 이 타스크를 시작하기 전에 첫 번째 실습에서 시작한 템플리트 전개가 완료되었는지 확인하십시오. 

1. Azure 포털에서 새로 제공된 Azure Recovery Services 자격 증명 **vaultaz1010102** 의 블레이드로 이동합니다.

1. **vaultaz1010102** 블레이드에서 상단에 **+ 복제**를 클릭하고 다음 복제 설정을 구성합니다:

    - 소스: **Azure**

    - 원본 위치: 이 랩의 이전 연습에서 Azure VM을 배포한 동일한 Azure 지역

    - Azure 가상 시스템 배포 모델: **Resource Manager**

    - 원본 구독 :이 실습의 이전 실습에서 사용한 것과 동일한 Azure 구독

    - 원본 리소스 그룹: **az1010101-RG**

    - 가상 머신: **az1010101-vm**

    - 대상 위치: 구독에서 사용할 수 있고 이전 작업에서 Azure VM을 배포한 지역과 다른 Azure 영역의 이름입니다. 가능하면 Site Resovert 자격 증명 자격 증명 모음을 배포한 것과 동일한 Azure 지역을 사용합니다.

    - 대상 리소스 그룹: **(신규) az1010101-RG-asr**

    - 대상 가상 네트워크: **(신규) az1010101-vnet-asr**

    - 캐시 스토리지 계정: 기본 설정

    - 복제 관리 디스크: **(신규) 1개 프리미엄 디스크, 0개 표준 디스크**

    - 대상 가용성 집합: **해당 없음**

    - 복제 정책: 새 복제 정책의 이름 **12-hour-retention-policy**

    - 복구 지점 보존: **12 시간**

    - 앱 일관된 스냅샷 빈도: **6 시간**

    - 다중 VM 일관성: **아니요**

1. **설정 구성** 블레이드에서 대상 리소스 만들기를 시작하고 **복제 사용** 블레이드로 리디렉션될 때까지 기다립니다.

1. **복제 사용** 블레이드에서복제를 사용하도록 설정합니다.


#### 작업 2: Azure VM 복제 설정 검토

1. Azure 포털에서 **vaultaz1010102 - 복제된 항목** 블레이드로이동합니다.

1. **vaultaz1010102 - 복제된 항목** 블레이드에서 Azure VM을 나타내는 **az1010101-vm** 항목이 있는지 확인하고 **복제 상태**가 **정상**인지 확인하고 **상태**가 **복제를 사용하도록 설정**하는지 확인합니다.

    > **참고**: **vaultaz1010102 - 복제된 항목** 블레이드에 **az1010101-vm**이 나타날 때까지 몇 분 정도 기다려야 할 수 있습니다.

1. **vaultaz1010102 - 복제된 항목** 블레이드에서 **az1010101-vm** Azure VM의 복제된 항목 블레이드를 표시합니다.

1. **az1010101-vm** 복제된 항목 블레이드에서 상태 및 **상태**, **장애 조치(Failover) 준비**, **최신 복구 지점** 및 **인프라 보기** 섹션을 검토합니다. **장애 조치(Failover)**와 **테스트 장애 조치(failover)** 도구 모음 아이콘을 참고합니다.

   > **참고**: 이 작업의 나머지 단계는 선택 사항이며 채점되지 않습니다. 

1. 시간이 허용되는 경우 복제 상태가 **100% 동기화**가 될 때까지 기다립니다. 이 경우 90분이 더 걸릴 수 있습니다. 

1. **RPO** 의 값과 **크래시 일관성** 및 **앱 일치** 복구 지점을 검사합니다. 

1. **az1010101-vnet-asr** 가상 네트워크에 대한 테스트 장애 조치(failover)를 수행합니다.

> **결과**: 이 연습을 완료한 후 Azure VM의 복제를 구성하고 Azure VM 복제 설정을 검토했습니다.


### 연습 3: 랩 리소스 삭제

#### 작업 1: Cloud Shell 열기

1. Azure 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. Cloud Shell 인터페이스에서 **Bash**를 선택합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter**를 눌러 이 랩에서 생성한 모든 리소스 그룹을 나열합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1010')].name" --output tsv
   ```

1. 출력된 결과가 이 랩에서 생성한 리소스 그룹만 포함되어 있는지 확인합니다. 이 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제하기

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter**를 눌러 이 랩에서 생성한 모든 리소스 그룹을 삭제합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1010')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

   > **참고**: "...cannot perform delete operation because following scope(s) are locked..."와 유사한 오류가 발생하면 다음 단계를 실행하여 삭제를 막는 리소스의 잠금을 제거해야 합니다.:
   > ```sh
   > lockedresource=$(az resource list --resource-group az1010101-RG-asr --resource-type Microsoft.Compute/disks --query "[?starts_with(name,'az10101')].name" --output tsv)
   > az disk revoke-access -n $lockedresource --resource-group az1010101-RG-asr
   > lockid=$(az lock show --name ASR-Lock --resource-group az1010101-RG-asr --resource-type Microsoft.Compute/disks --resource-name $lockedresource --output tsv --query id)
   > az lock delete --ids $lockid
   > az group list --query "[?starts_with(name,'az10101')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   >```

1. **Cloud Shell** 명령 프롬프트를 닫습니다.

> **결과**: 이 연습을 완료한 후 이 랩에서 사용된 리소스 그룹을 제거했습니다.
