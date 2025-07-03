# exxon
Exxon_SourceCode
#How to clean Data
อันดับแรกพวกเรา clean file inbound , outbound , Inventory
โดยจัดการกับ duplicate data โดยการยุบให้เหลือ 1
จัดการ missing value ด้วยการลบทิ้งเนื่องจากมีน้อย
จัดการ outlier ด้วยการแยกไฟล์ csv ออกเป็น 2 ไฟล์
ไฟล์ที่ถูก clean outlier จนหมด กับ ไฟล์ ที่มีแต่ outlier

#Add Feature
1.inbound join ร่วมกับ materail
2.เพิ่ม เวลา เรื่องหมดอายุ
3.merge inbound outbound 
4.เพิ้ม column เดือนที่ควร outbound 
5.แปลงสกุลเงินให้เป็น dolla
6.หา row ไหนบ้างที่ ค่าcontainer รับไม่ไหว
แยก date เดือน/ปี groupby netquality ตามเดือน ตาม week แปลงเป็น กิโลตัน
วันหยุดหรือไม่ อิงตามไฟล์ wh_calender ตามประเทศนั้นๆ

#อธิบายว่าไฟล์แต่ละไฟลทำงานยังไง
1.outbound_prepare.ipynb ทำการ clean data outbound.csv และคืนไฟล์ outbound_clean

2.Inbound_Cleaned(All_dup).ipynp ทำการ clean data inbound.csv และคืนไฟล์ 'Inbound_Cleaned(All dup).csv'

3.inventory_prepare.ipynb ทำการ clean data inventory_prepare.csv และคืนไฟล์ 2 ไฟล์
inventory_perfect.csv อยู่ในช่วง IQR และ inventory_outler.csv ที่เป็น outlier

4.Feature_Engineering_andMore.ipynb อ่านไฟล์ 'Inbound_Cleaned(All dup) (4).csv' และ 'MaterialMaster.csv' ทำการmerge กัน
และอ่านไฟล์ outbound_clean.csv , 'OperationCost.csv' หาว่า NET_QUANTITY_MT เยอะกว่า 24.75ไหม
ทำการ คำนวน tranfer cost และคืนค่าไฟล์ outbound_group_withcost.csv ออกมา
ทำการ groupby เวลา inbound กับ NET_QUANTITY_MT และ ทำให้เป็นหน่วยกิโล
คืนไฟล์ inbound_group.csv และหาวันหยุด คืนไฟล์ 'outbound_mat_holiday_byweek.csv'

5.outbound_mat_holiday_byweek.csv นี่คือไฟล์ที่จะนำไปทำ ML ต่อ
