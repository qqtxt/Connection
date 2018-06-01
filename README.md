# Connection
PDO  mysql数据库连接库

具体MySQL/Connection用法
// 初始化db连接
$db = new \Workerman\MySQL\Connection('host', 'port', 'user', 'password', 'db_name');

// 获取所有数据
$db->select('ID,Sex')->from('Persons')->where('sex= :sex AND ID = :id')->bindValues(array('sex'=>'M', 'id' => 1))->query();
//等价于
$db->select('ID,Sex')->from('Persons')->where("sex= 'M' AND ID = 1")->query();
//等价于
$db->query("SELECT ID,Sex FROM `Persons` WHERE sex='M' AND ID = 1");


// 获取一行数据
$db->select('ID,Sex')->from('Persons')->where('sex= :sex')->bindValues(array('sex'=>'M'))->row();
//等价于
$db->select('ID,Sex')->from('Persons')->where("sex= 'M' ")->row();
//等价于
$db->row("SELECT ID,Sex FROM `Persons` WHERE sex='M'");


// 获取一列数据
$db->select('ID')->from('Persons')->where('sex= :sex')->bindValues(array('sex'=>'M'))->column();
//等价于
$db->select('ID')->from('Persons')->where("sex= 'F' ")->column();
//等价于
$db->column("SELECT `ID` FROM `Persons` WHERE sex='M'");

// 获取单个值
$db->select('ID')->from('Persons')->where('sex= :sex')->bindValues(array('sex'=>'M'))->single();
//等价于
$db->select('ID')->from('Persons')->where("sex= 'F' ")->single();
//等价于
$db->single("SELECT ID FROM `Persons` WHERE sex='M'");

// 复杂查询
$db->select('*')->from('table1')->innerJoin('table2','table1.uid = table2.uid')->where('age > :age')->groupBy(array('aid'))->having('foo="foo"')->orderByASC/*orderByDESC*/(array('did'))
->limit(10)->offset(20)->bindValues(array('age' => 13));
// 等价于
$db->query('SELECT * FROM `table1` INNER JOIN `table2` ON `table1`.`uid` = `table2`.`uid`
WHERE age > 13 GROUP BY aid HAVING foo="foo" ORDER BY did LIMIT 10 OFFSET 20');

// 插入
$insert_id = $db->insert('Persons')->cols(array(
    'Firstname'=>'abc',
    'Lastname'=>'efg',
    'Sex'=>'M',
    'Age'=>13))->query();
等价于
$insert_id = $db->query("INSERT INTO `Persons` ( `Firstname`,`Lastname`,`Sex`,`Age`)
VALUES ( 'abc', 'efg', 'M', 13)");

// 更新
$row_count = $db->update('Persons')->cols(array('sex'))->where('ID=1')
->bindValue('sex', 'F')->query();
// 等价于
$row_count = $db->update('Persons')->cols(array('sex'=>'F'))->where('ID=1')->query();
// 等价于
$row_count = $db->query("UPDATE `Persons` SET `sex` = 'F' WHERE ID=1");

// 删除
$row_count = $db->delete('Persons')->where('ID=9')->query();
// 等价于
$row_count = $db->query("DELETE FROM `Persons` WHERE ID=9");

// 事务
$db->beginTrans();
....
$db->commitTrans(); // or $db->rollBackTrans();
