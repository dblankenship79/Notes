## Defining Entities

Lets start by defining our base class for our Entity Framework Entities.  Add the BaseEntity class to our Models folder.

``` csharp
public abstract class BaseEntity
{
  public int Id { get; set; }
  public Guid TenantId { get; set; }
  public bool isDeleted { get; set; }
}
```
## Tenant Provider

Create our interface for our Tenant.

``` csharp
public interface ITenantProvider 
{
  Guid GetTenantId();
}
```

And our implementation
``` csharp
public class TenantProvider : ITenantProvider
{
  public Guid GetTenantId()
  {
    return Guid.NewGuid();
  }
}
```

## Data Context
``` csharp
public class ClientContext : DbContext
{
  private Guid _tenantId;

  public ClientContext(ITenantProvider tenantProvider)
  {
    _tenantId = tenantProvider.GetTenantId();
  }
}
```