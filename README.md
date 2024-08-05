# LineBot + Open-Weather-API
使用Google App Script寫腳本，抓取氣象局API氣象資料並分析，再將其資料透過LineBot API傳送給使用著。
# STEP 1 創建Google App Script專案
![image](https://github.com/BillShiau0720/LineBot-OpenWeather_API/blob/main/Step1.jpg)
# STEP 2 抓取氣象局API氣象資料並分析(此範例為高雄市，如要別的城市就需要替換search_code)
```js
    var apiKey = "API授權碼";
    var search_code = "F-D0047-067";
  var weathrerDay = year+"-"+month+"-"+date;
  var rural_areas = "高雄市鄉區 ex:岡山區";
  //不能使用http，會無回應或報錯
  var apiCall = "https://opendata.cwa.gov.tw/api/v1/rest/datastore/" search_code + "?Authorization="+ apiKey + "&limit=1&format=JSON&locationName="+ rural_areas + "&elementName=&timeFrom=" + weathrerDay+"T06%3A00%3A00&timeTo=" + weathrerDay + "T18%3A00%3A00";
  
  var response = UrlFetchApp.fetch(apiCall);
  var data = JSON.parse(response.getContentText());
  //Logger.log(response.getResponseCode());
  if (data.cod === "404") { // City not found
    var errorMsg = "發生錯誤";
    return errorMsg;
  }
  if(response.getResponseCode() == 200){
    var text = data["success"];
    var today = data["records"];
    var weather = today["locations"][0];
    var weather2 = weather["location"][0];
    var weather3 = weather2["weatherElement"][6];
    var weather4 = weather3["time"][0];
    var weather5 = weather4["elementValue"][0];
    var weatherDesc = weather5["value"];
    var weather6 = weather4["elementValue"][1];
    var weatherDescCode = weather6["value"];
    let arr = []; 
    arr[0] = weatherDesc;
    arr[1] = weatherDescCode;
    //Logger.log(arr[0]+","+arr[1]);
    return arr;
  }
    ```
