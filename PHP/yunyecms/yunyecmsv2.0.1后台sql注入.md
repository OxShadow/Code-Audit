yunyecms后台存在sql注入漏洞

![image-20250330221808992](image/image-20250330221808992.png)

修改部门名字

![image-20250330221944700](image/image-20250330221944700.png)

id处存在注入

![image-20250330222432903](image/image-20250330222432903.png)

代码分析：

定位到core/admin/department.php

```
public function department_add(){
 if($_POST["yyact"]=="edit"){
			       $id=$_POST["id"];
			       $olddepartmentname=$_POST["olddepartmentname"];
				   $this->edit_admin_department($id,$departmentname,$olddepartmentname,$status);
			  }			  
```

直接从post接受一个id参数

```
if($departmentname!=$olddepartmentname){
			   $num=$this->db->GetCount("select count(*) as total from `#yunyecms_department` where departmentname='$departmentname' and departmentid<>$id limit 1")
```

会被输入到sql语句中

![image-20250330224027305](image/image-20250330224027305.png)

只需要更改的时候不重复名字即可

