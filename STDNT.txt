import java.io.*;
import java.util.*;
import java.sql.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

class DBConnect
{  
    private Connection con;
    private Statement st;
    private ResultSet rs;  
    private String sql;
    public DBConnect()
    {
      try
      {  
       Class.forName("com.mysql.jdbc.Driver"); 
       con=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","");
       st=con.createStatement(); 
       sql = "CREATE TABLE IF NOT EXISTS STUDENT " +
                   "(rollno INTEGER not NULL, " +
                   " name VARCHAR(255), " + 
                   " age INTEGER, " + 
                   " marks INTEGER, " + 
                   " PRIMARY KEY ( rollno ))";
       st.executeUpdate(sql);
      }
      catch(Exception e)
      {
        System.out.println("Error :"+e);
      }
    }
    public void inser()
    {
        try
        { 
      System.out.println("Enter rollno");
      Scanner c=new Scanner(System.in);
      int r=c.nextInt();
      System.out.println("Enter name");
      Scanner s=new Scanner(System.in);
      String n=s.nextLine();
      System.out.println("Enter age");
      Scanner q=new Scanner(System.in);
      int a=q.nextInt();
      System.out.println("Enter marks");
      int m=q.nextInt();
         sql = "INSERT INTO STUDENT " +
                   "VALUES ("+r+", '"+n+"', "+a+", "+m+")";
      st.executeUpdate(sql);
      System.out.println("Inserted records into the table");
        }
        catch(Exception e)
        {
            System.out.println("Error :"+e);
        }
    }
    public void dlt()
    {
        try
        {
            sql = "TRUNCATE STUDENT";
            st.executeUpdate(sql); 
            sql = "DELETE FROM STUDENT";
            st.executeUpdate(sql);
            System.out.println("Deleted all details drom the table");
        }
        catch(Exception e)
        {
            System.out.println("Error :"+e);
        }
    }
    public void getData()
    {
      try
      {
        int d=0;
        String query="select * from STUDENT";
        rs=st.executeQuery(query);
        System.out.println("Records of student table");
        while(rs.next())
        {
            d++;
            String rollno=rs.getString("rollno");
            String name=rs.getString("name");
            String age=rs.getString("age");
            String marks=rs.getString("marks");
            System.out.println("Rollno : "+rollno+" Name : "+name+" Age : "+age+" Marks : "+marks);
        }
        if(d==0)
        {
            System.out.println("TABLE IS EMPTY");
        }
      }
      catch(Exception e)
      {
        System.out.println("Error :"+e);
      }
     }
     public void searchData()
     {
        try
        {
            int f=0;
            System.out.println("Enter the rollno to be searched");
            Scanner in=new Scanner(System.in);
            int r=in.nextInt();
            String query="select * from STUDENT where rollno="+r;
            rs=st.executeQuery(query);
            while(rs.next())
            {
             f++;
            String rollno=rs.getString("rollno");
            String name=rs.getString("name");
            String age=rs.getString("age");
            String marks=rs.getString("marks");
            System.out.println("Rollno : "+rollno+" Name : "+name+" Age : "+age+" Marks : "+marks);
            }
            if(f==0)
            {
                System.out.println("ROLLNO NOT FOUND IN TABLE STUDENT");
            }
        }
        catch(Exception e)
        {
            System.out.println("Error :"+e);
        }
     }
   }
class BtnPgm extends JFrame implements ActionListener
{
    private JButton button1,button2,button3,button4;
    public BtnPgm()
    {
        super("Student Record");
        setLayout(new FlowLayout());
        button1=new JButton("Insert");
        button2=new JButton("Search by rollno");
        button3=new JButton("Print all details");
        button4=new JButton("Delete all details");
        add(button1);
        add(button2);
        add(button3);
        add(button4);
        button1.addActionListener(this);
        button2.addActionListener(this);
        button3.addActionListener(this);
        button4.addActionListener(this);
        setSize(300,300);
        setVisible(true);
    }
    public void actionPerformed(ActionEvent e)
    {
        if(e.getSource()==button1)
        {
          DBConnect connect=new DBConnect();
          connect.inser();
        }
          if(e.getSource()==button2)
        {
          DBConnect connect=new DBConnect();
          connect.searchData();
        }
        if(e.getSource()==button3)
        {
          DBConnect connect=new DBConnect();
          connect.getData();
        }
         if(e.getSource()==button4)
        {
          DBConnect connect=new DBConnect();
          connect.dlt();
        }
    }
}
class Student
{ 
    public static void main (String args[]) 
    { 
      BtnPgm b=new BtnPgm();
    }
}  