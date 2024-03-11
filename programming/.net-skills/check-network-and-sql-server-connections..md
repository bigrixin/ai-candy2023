# Check Network and SQL server connections.



```
// Check network

 private bool isNetworkConnectionAvailable()
 {
     bool _success = false;
     string[] sitesList = { "www.google.com", "www.yahoo.com", "www.youtube.com" };
     Ping ping = new Ping();
     PingReply reply;
     label1.Text = "";
     int notReturned = 0;
     try
     {
         for (int i = 0; i < sitesList.Length; i++)
         {
             reply = ping.Send(sitesList[i], 10);
             if (reply.Status == IPStatus.Success)
             {
                 _success = true;
                 Console.WriteLine("Ping " + sitesList[i] + " OK !");
                 break;
             }
             else
             {
   
                 Console.WriteLine("Failed to ping " + sitesList[i] );
       
                 notReturned += 1;
             }
         }
     }
     catch
     {
         _success = false;
         //throw new Exception(ex.Message);
     }
     return _success;
 }
```

```
// Check SQL Server

private bool checkSQLServerConection(string ConnectionString)
{
    try
    {
        using (var connection = new SqlConnection(ConnectionString))
        {
            var query = "select 1";
            Console.WriteLine("Executing: {0}", query);

            var command = new SqlCommand(query, connection);

            connection.Open();
            Console.WriteLine("SQL connection successful");

            command.ExecuteScalar();
            Console.WriteLine("SQL Query execution successful");
            connection.Close();
            return true;
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("Failure: {0}", ex.Message);
        LogBox?.AppendText(ex.StackTrace);
    }
    return false;
}
```

```
// Delay process


/// ------- delay check network  --------------------
private void delayCheckNetwork(int delayTimeSeconds)
{
    connect_network_timer.Interval = delayTimeSeconds * 1000;  //time in milliseconds
    connect_network_timer.Enabled = true;
    connect_network_timer.Tick += new EventHandler(check_network_timer_Tick);
    var aa = connect_network_timer.Tag;
}

private void check_network_timer_Tick(object sender, EventArgs e)
{
    button_check_network.Text = TryCheckingNetworkTimes.ToString();
    if (IsNetworkConnected || TryCheckingNetworkTimes < 1)
    {
        connect_network_timer.Stop();
        button_check_network.Text = "Check Internet";
    }
    else
    {
       Console.WriteLine("Check network connection - " + TryCheckingNetworkTimes.ToString() + " times left.\n");
        button_check_network_Click(sender, e);
        TryCheckingNetworkTimes--;

        Thread.Sleep(DelayCheckNetworkSeconds * 1000);  //time in milliseconds
    }
}

/// ------- delay check sql --------------------
private void delayCheckSql(int delayTimeSeconds)
{
    connect_sql_timer.Interval = delayTimeSeconds * 1000;  //time in milliseconds
    connect_sql_timer.Enabled = true;
    connect_sql_timer.Tick += new EventHandler(check_sql_timer_Tick);
}

private void check_sql_timer_Tick(object sender, EventArgs e)
{
    button_check_sql.Text = TryCheckingSQLTimes.ToString();
    if (IsSQLConnected || TryCheckingSQLTimes < 1)
    {
        connect_sql_timer.Stop();
        button_check_sql.Text = "Check SQL";
    }
    else
    {
        Console.WriteLine("Check SQL Server connection - " + TryCheckingSQLTimes.ToString() + " times left. \n");
        button_check_sql_Click(sender, e);
        TryCheckingSQLTimes--;
        Thread.Sleep(DelayCheckSQLSeconds * 1000);  //time in milliseconds
    }
}

```
