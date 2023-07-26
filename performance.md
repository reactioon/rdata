# RDATA
RDATA is a Data Layer that aim to be lightweight, fast, easy distribution and scalable. 

The main goal of this project is work with large amount of data inside on Reactioon services/tools. But on reactioon ecossystem the users can create their own tools, and on RDATA, can create anything.

## Performance
The main goal of RDATA is the speed, so we are looking for high speed with low resources usage. But to get the best of enviroment, the user can help RDATA improve the search of data working with indexes and variables. Check below some aspects that the developer needs to be careful.

1. **Search data**  
When the book is too big, it's hard to find the data. So, create index for field that will be used in search (you can create multiples indexes). The city field inside of an document is a good example of index that will be used in data search.

2. **Shards**  
Don't use an high number of shards (in our case, we call 'pages') in your rdata.yaml config, option BOOK_PAGES. Pages mean the capacity of an book, so an book with a lot of pages is not improved for performance. The number of pages is flexible, so you can change when you want. The recomendation is start with a number of pages <= 100 to improve performance in most of scenarios.

3. **Logs**  
Logs is a important piece of RDATA to see what is happening inside of your instance, this is better for management. But this option reduces the perfomance, if you are looking for more performance than management, disable logs.

4. **Data Descentralization**  
The main concept of RDATA is the descentralization of data enviroment. You can create multiples instances of RDATA inside of the same machine to each action of your software, multiples collections or multiples instances. If you are working with an cloud, just create an snapshot and add the address in your load balancer.

5. **Heavy work-loads**  
If you are working with an high amount of requests or large amounts of data, you can segregate collection to an specific action over network. This mean that when you want execute an action you can add it inside of instance that only execute this things related to their context.


The whole concept of RDATA is to offer to our tools an solution that can store and serve data fast, too fast.

That's all!  
Thanks

José Wilker