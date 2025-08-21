## Diskpart 명령 예시 (D: 삭제 → C: 확장)

⚠️ 실행 전 반드시 D: 데이터 백업하세요. 삭제 시 복구 불가입니다.

```
diskpart
list disk
select disk 0             # 대상 디스크 선택
list partition
select partition <D드라이브 번호> 
delete partition override # D: 삭제 (데이터 완전 삭제됨)
select partition <C드라이브 번호>
extend
exit

```