## simple test for apache's DbUtils project

## Notion

When use the apache DbUtils ,one must take care the transaction problem.
Basicalliy,if u specify connection through param,u have to manually invoke submit() and close() method.
Just like below codes in sevice layor.
### Dao layor:
		QueryRunner runner=new QueryRunner();
		Long result=runner.insert(DbUtils.getConnection(),sql, new ScalarHandler<Long>(),"ivanka Trump",30,false,false);
	
	
### Service layor:
		 public void insert() throws SQLException{
				 try{
					 WomanDao dao=new WomanDao();
					 dao.insert();
					 DbUtils.submit();
				 }catch(Exception e){
					 e.printStackTrace();
					 DbUtils.rollback();
				 }finally{
					 DbUtils.closeConnection();
				 }
			 }

oppositely,if u specify DataSource through below code,then DbUtils will autocommit and autoClose the connection.
### Dao layor:
	QueryRunner runner=new QueryRunner(dataSource);
	Long result=runner.insert(sql, new ScalarHandler<Long>(),"ivanka Trump",30,false,false);

