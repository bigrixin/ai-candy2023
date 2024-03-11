# Object comparison



{% code fullWidth="true" %}
```
// Json comparison, used for two list<object>

    private bool jsonCompare(object oldData, object newData)
    {
        string myself = JsonConvert.SerializeObject(oldData);
        string other = JsonConvert.SerializeObject(newData);
        return myself.Equals(other);  
    }

```
{% endcode %}



{% code fullWidth="true" %}
```
// used for two class

    private bool DeepCompare(this object obj, object another)
    {
        if (ReferenceEquals(obj, another)) 
            return true;
        if ((obj == null) || (another == null)) 
            return false;
        //Compare two object's class, return false if they are difference
        if (obj.GetType() != another.GetType()) 
            return false;

        //properties: int, double, DateTime, etc, not class
        if (!obj.GetType().IsClass) 
            return obj.Equals(another);

        var result = true;

        //Get all properties of obj, compare each other
        foreach (var property in obj.GetType().GetProperties())
        {
            var objValue = property.GetValue(obj);
            var anotherValue = property.GetValue(another);
            if (!objValue.Equals(anotherValue)) 
                result = false;
        }
        return result;
    }

```
{% endcode %}
