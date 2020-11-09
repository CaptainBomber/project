package com.company;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

import java.util.Calendar;
import java.util.concurrent.TimeUnit;
import java.util.spi.CalendarDataProvider;

public class Calander {
    static int target_day = 0;
    static int target_month = 0;
    static int target_year = 0;
    static int current_day = 0;
    static int current_month = 0;
    static int current_year = 0;
    static  int jump = 0;
    static boolean incriment = true;
    public static void main(String[] args) throws InterruptedException {
        // target date
        //current date
        //
    String td = "31/12/2020";
    //get current date
        getcdateyear();
        System.out.println(current_day+ " "+current_month +" " +current_year);
     getTdateyear(td);
     System.out.println(target_day+ " "+target_month +" " +target_year);
     jump();
        System.out.println(jump);
        System.out.println(incriment);
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.spicejet.com/");
        driver.manage().timeouts().implicitlyWait(10l, TimeUnit.SECONDS);
        driver.findElement(By.xpath("//*[@id='flightSearchContainer']/div[4]/button")).click();
        for(int i = 0; i< jump;i++){
            if(incriment)
                driver.findElement(By.xpath("//*[@id='ui-datepicker-div']/div[2]/div/a/span")).click();
            else
                driver.findElement(By.xpath("//*[@id='ui-datepicker-div']/div[1]/div/a/span"));
            Thread.sleep(1000);
        }
     driver.findElement(By.linkText(Integer.toString(target_day))).click();
    }
    public static void getcdateyear(){
        Calendar c = Calendar.getInstance();
        current_day = c.get(Calendar.DAY_OF_MONTH);
        current_month = c.get(Calendar.MONTH)+1;
        current_year = c.get(Calendar.YEAR);
    }
    public static void getTdateyear(String date){
        int fi = date.indexOf("/");
        int li = date.lastIndexOf("/");
        String day  = date.substring(0,fi);
        target_day = Integer.parseInt(day);
        String month = date.substring(fi+1,li);
        target_month = Integer.parseInt(month);
        String year = date.substring(li+1,date.length());
        target_year = Integer.parseInt(year);
    }
    public static void jump()
    {
        if (target_month - current_month > 0){
            jump = target_month - current_month;
        }else {
            jump = current_month - target_month;
            incriment = false;
        }
    }
}
