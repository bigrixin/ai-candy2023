# Add ID manually



{% code fullWidth="true" %}
```
// Max Id

        public int GetMaxId()
        {
            var entityType = _context.Model.FindEntityType(typeof(T));
            var primaryKey = entityType.FindPrimaryKey().Properties[0];
            var primaryKeyPropertyName = primaryKey.Name;

            var parameter = Expression.Parameter(typeof(T), "e");
            var property = Expression.Property(parameter, primaryKeyPropertyName);
            var convert = Expression.Convert(property, typeof(int?));
            var lambda = Expression.Lambda<Func<T, int?>>(convert, parameter);

            var maxId = _context.Set<T>().Max(lambda);
            return maxId ?? 0; // if null, return 0
        }

        public async Task ManualAddAsync(T entity)
        {
            // Get the current maximum ID
            var maxId = GetMaxId();

            // Assuming 'Id' is a property of type int
            var idProperty = entity.GetType().GetProperty("Id");
            if (idProperty != null && idProperty.CanWrite)
            {
                idProperty.SetValue(entity, maxId + 1);
            }

            // Add the entity to the context
            await _context.AddAsync(entity);
        }

```
{% endcode %}
