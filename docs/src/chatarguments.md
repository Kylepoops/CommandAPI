# Chat arguments

## Chat color argument

The `ChatColorArgument` class is used to represent a given chat color (e.g. red or green)

### Example - Username color changing plugin

```java
LinkedHashMap<String, Argument> arguments = new LinkedHashMap<>();
arguments.put("chatcolor", new ChatColorArgument());

CommandAPI.getInstance().register("namecolor", arguments, (sender, args) -> {
	Player player = (Player) sender;
	ChatColor color = (ChatColor) args[0];
	player.setDisplayName(color + player.getDisplayName());
});
```
-----

# Spigot-based chat arguments

> **Developer's Note:**
>
> The two following classes, `ChatComponentArgument` and `ChatArgument` depend on a Spigot based server. This means that these arguments will not work on a non-Spigot based server, such as CraftBukkit. If you use this class on a non-Spigot based server, it will throw a `SpigotNotFoundException`
>
> Spigot based servers include, but are not limited to:
> * [Spigot](https://www.spigotmc.org/)
> * [PaperSpigot](https://papermc.io/)
> * [TacoSpigot](https://tacospigot.github.io/)

## Chat component argument

The `ChatComponentArgument` class accepts raw chat-based JSON as valid input. Despite being regular JSON, it _must_ conform to the standard declared [here](https://minecraft.gamepedia.com/Commands#Raw_JSON_text), which consists of JSON that has a limited subset of specific keys (In other words, you can have a JSON object that has the key `text`, but not one that has the key `blah`).

This is converted into Spigot's `BaseComponent[]`, which can be used for the following:

- Broadcasting messages to all players on the server using:

  ````java
  Bukkit.getServer().spigot().broadcast(BaseComponent[]);
  ````

- Adding and setting pages to books using `BookMeta`:

  ```java
  BookMeta meta = // ...
  meta.spigot().addPage(BaseComponent[]);
  meta.spigot().setPage(int, BaseComponent[]);
  ```

- Sending messages to `Player` objects:

  ```java
  Player player = // ...
  player.spigot().sendMessage(BaseComponent[]);
  ```

- Sending messages to `CommandSender` objects:

  ```java
  CommandSender sender = // ...
  sender.spigot().sendMessage(BaseComponent[]);
  ```

### Example - Book made from raw JSON

```java
LinkedHashMap<String, Argument> arguments = new LinkedHashMap<>();
arguments.put("contents", new ChatComponentArgument());

CommandAPI.getInstance().register("makebook", arguments, (sender, args) -> {
	if (sender instanceof Player) {
		Player player = (Player) sender;
		BaseComponent[] arr = (BaseComponent[]) args[0];
		
        //Create book
		ItemStack is = new ItemStack(Material.WRITTEN_BOOK);
		BookMeta meta = (BookMeta) is.getItemMeta(); 
		meta.spigot().addPage(arr);
		is.setItemMeta(meta);
		
        //Give player the book
		player.getInventory().addItem(is);
	}
});
```

## Chat argument
