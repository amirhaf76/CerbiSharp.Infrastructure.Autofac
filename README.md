# Cerbisharp.Infrastructure.Autofact

This is usage of `BaseDIModule<>` in [BookShop-V2](https://github.com/amirhaf76/Book-ShopV2#).

## BookShopDIModule

```C#
namespace BookShop.Core.DIModule
{
    public class BookShopDIModule : BaseDIModule<BookShopDIModule>
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder
                .Register<DbContext>(c => c.Resolve<BookShopDbContext>())
                .AsSelf();
                
            base.Load(builder);
        }
    }

    public static class BookShopDIModuleExtension
    {
        public static ContainerBuilder AddBookShopDIModule(this ContainerBuilder builder)
        {
            var assemply = Assembly.GetAssembly(typeof(BookShopDIModule));

            builder.RegisterAssemblyModules(assemply);

            return builder;
        }
    }
}
```

## Injecting Dependency

First we need to set Autofac as service provider factory.

```C#
builder.Host.UseServiceProviderFactory(new AutofacServiceProviderFactory());
```

Now we can add `BookShopDIModule` to `IContainerBuilder`

```C#
builder.Host.ConfigureContainer<ContainerBuilder>(builder =>
{
    builder.AddBookShopDIModule();
});
```
