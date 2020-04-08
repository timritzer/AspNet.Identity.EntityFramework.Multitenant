ASP.NET Core Identity EntityFramework Multitenant
============================================

Multi-tenant support for ASP.NET Core Identity using Entity Framework

This library is a fork of the original AspNet.Identity.EntityFramework.Multitenant updated to support Identity in .Net Core

## How to use multi-tenancy

To use this library the `TenantId` property on the `MultitenantUserStore` needs to be set for the relevant tenant. How and when this is set is determined by the application.

Thereafter, the `UserManager` (which has a dependency on the `UserStore`) can be used and it will only find and update users for the specified tenant.

## What has been multi-tenanted?

The intention is to provide a means to allow for identical users (uniquely keyed upon email/username/external login) to register and authenticate multiple times under different tenants, but still remaining unique within a tenant, all within the same database and entity context.

The two entities that have been extended to support multi tenancy are [`IdentityUser`](http://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityuser.aspx "IdentityUser Class") and [`IdentityUserLogin`](http://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityuserlogin.aspx "IdentityUserLogin Class"), both of which have had the `TenantId` property added.

Following the same patterns of [Microsoft ASP.NET Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/) there is both a generic and non generic equivalent proved:

1. Use or extend [`MultitenantIdentityUser`](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/blob/master/src/AspNet.Identity.EntityFramework.Multitenant/MultitenantIdentityUser.cs "MultitenantIdentityUser class") and [`MultitenantIdentityUserLogin`](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/blob/master/src/AspNet.Identity.EntityFramework.Multitenant/MultitenantIdentityUserLogin.cs "MultitenantIdentityUserLogin class") if you want the `TenantId` to be a string
1. Extend [`MultitenantIdentityUser<TKey, TTenantKey, TLogin, TRole, TClaim>`](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/blob/master/src/AspNet.Identity.EntityFramework.Multitenant/MultitenantIdentityUser.Generic.cs "MultitenantIdentityUser generic class") and [`MultitenantIdentityUserLogin<TKey, TTenantKey>`](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/blob/master/src/AspNet.Identity.EntityFramework.Multitenant/MultitenantIdentityUserLogin.Generic.cs "MultitenantIdentityUserLogin generic class") if you want custom `TenantId`.

The [`UserStore<TUser>`](http://msdn.microsoft.com/en-us/library/dn315446.aspx "UserStore generic Class") has been extended to have a `TenantId` property and to override the necessary methods to implement multi-tenancy. The `TenantId` needs to be specified directly otherwise an [`InvalidOperationException`](http://msdn.microsoft.com/en-us/library/system.invalidoperationexception.aspx "InvalidOperationException class") will be thrown. See the [`MultitenantUserStore<TUser, TRole, TKey, TTenantKey, TUserLogin, TUserRole, TUserClaim>`](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/blob/master/src/AspNet.Identity.EntityFramework.Multitenant/MultitenantUserStore.Generic.cs "MultitenantUserStore generic class") for more details.

Lastly the [`IdentityDbContext<TUser>`](http://msdn.microsoft.com/en-us/library/dn468176.aspx "IdentityDbContext generic class") has been extended to add the necessary validation and model creation to implement multi-tenancy. See the [`MultitenantIdentityDbContext<TUser, TRole, TKey, TTenantKey, TUserLogin, TUserRole, TUserClaim>`](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/blob/master/src/AspNet.Identity.EntityFramework.Multitenant/MultitenantIdentityDbContext.Generic.cs "MultitenantIdentityDbContext generic class") for more details.

## Example

Two simple sites that make use the package to allow for multi-tenancy are provided as example:

1. [Vanilla Implementation](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/tree/master/src/Examples/VanillaImplementation) which is the standard MVC5 template project that uses a string primary keys.
  * The changes necessary to provide multi-tenancy have been isolated to this [commit](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/commit/2360ea55ac89195c645e130927835f4a4bea3e58).
1. [Integer Primary Key Implementation](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/tree/master/src/Examples/IntegerPkImplementation) which is making a few more customisations to use integer primary keys
  * The changes necessary to provide multi-tenancy have been isolated to this [commit](https://github.com/JSkimming/AspNet.Identity.EntityFramework.Multitenant/commit/9d853306d5d5e25750f6875da15afe9f5ead73f6).
