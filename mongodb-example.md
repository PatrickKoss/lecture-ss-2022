## Start using a databse
```bash
docker-compose exec mongodb mongo -u test -p test
```
```bash
$ use amazon
```

## Insert customer
```bash
db.customer.insert({"_id" : ObjectId("6249eb54f7e50baefd9518c8"), "firstName": "Patrick", "lastName": "Koss", "email": "patrick@koss.de", "paymentDetails": {"payPal": ["patrick@koss.de"]}, "addresses": [{"zipCode": "70192", "street": "random street 1", "city": "Heilbronn"}]});
db.customer.insert({"_id" : ObjectId("624b274af7e50baefd9518d2"), "firstName": "Sophie", "lastName": "S", "email": "sophie@s.de", "paymentDetails": {"payPal": ["sophie@s.de"]}, "addresses": [{"zipCode": "70192", "street": "random street 1", "city": "Heilbronn"}]});
db.customer.insert({"_id" : ObjectId("624b274bf7e50baefd9518d3"), "firstName": "Tobias", "lastName": "T", "email": "tobi@t.de", "paymentDetails": {"payPal": ["tobi@t.de"]}, "addresses": [{"zipCode": "70192", "street": "random street 1", "city": "Heilbronn"}]});
```

## Insert categories
```bash
db.category.insert({"_id" : ObjectId("624b2523f7e50baefd9518cd"), "name": "Cloths", "subcategories": []})
db.category.insert({"_id" : ObjectId("624b24eff7e50baefd9518cb"), "name": "Hardware Store", "subcategories": []})
db.category.insert({"_id" : ObjectId("624b2576f7e50baefd9518ce"), "name": "Beauty", "subcategories": []})
db.category.insert({"_id" : ObjectId("624b259cf7e50baefd9518cf"), "name": "Games", "subcategories": []})
db.category.insert({"_id" : ObjectId("624b25cdf7e50baefd9518d0"), "name": "Toys", "subcategories": []})
db.category.insert({"_id" : ObjectId("624b25e9f7e50baefd9518d1"), "name": "Computer Hardware", "subcategories": []})
```

## Insert Products
```bash
db.product.insert({"_id" : ObjectId("624b30e5f7e50baefd9518d6"), "name": "Apple IPhone Pro 13 MAX", "description": "Awesome smartphone", "pictures": [], "price": 1500.00, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 5.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 4.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 4.5}], "categories": [ObjectId("624b25e9f7e50baefd9518d1")]});
db.product.insert({"_id" : ObjectId("624b3506f7e50baefd9518d8"), "name": "Samsung S21", "description": "Awesome smartphone but from samsung", "pictures": [], "price": 1400.00, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 5.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 4.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 4.5, "recession": "awsome"}], "categories": [ObjectId("624b25e9f7e50baefd9518d1")]});
db.product.insert({"_id" : ObjectId("624b3506f7e50baefd9518d9"), "name": "Puma Socks", "description": "Cool socks", "pictures": [], "price": 55.95, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 5.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 3.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 3.5, "recession": "cool bro"}], "categories": [ObjectId("624b2523f7e50baefd9518cd")]});
db.product.insert({"_id" : ObjectId("624b3506f7e50baefd9518da"), "name": "Jack&Jones Tshirt", "description": "tshirt in black", "pictures": [], "price": 8.99, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 5.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 4.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 3.5, "recession": "tshirt is good"}], "categories": [ObjectId("624b2523f7e50baefd9518cd")]});
db.product.insert({"_id" : ObjectId("624b3506f7e50baefd9518db"), "name": "tesa", "description": "tesa", "pictures": [], "price": 8.99, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 3.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 3.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 3.5, "recession": "tesa works"}], "categories": [ObjectId("624b24eff7e50baefd9518cb")]});
db.product.insert({"_id" : ObjectId("624b3506f7e50baefd9518dc"), "name": "saw", "description": "very sharp saw", "pictures": [], "price": 31.87, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 3.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 4.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 3.0, "recession": "sharp enough"}], "categories": [ObjectId("624b24eff7e50baefd9518cb")]});
db.product.insert({"_id" : ObjectId("624b3506f7e50baefd9518dd"), "name": "hair dryer", "description": "drying hair like a boss", "pictures": [], "price": 25.00, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 3.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 4.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 3.0, "recession": "hot"}], "categories": [ObjectId("624b2576f7e50baefd9518ce")]});
db.product.insert({"_id" : ObjectId("624b3509f7e50baefd9518de"), "name": "tooth brush", "description": "brushing teeth is fun", "pictures": [], "price": 1.00, "createDate": ISODate(), "recessions": [{"customer": ObjectId("6249eb54f7e50baefd9518c8"), "rating": 2.0}, {"customer": ObjectId("624b274af7e50baefd9518d2"), "rating": 1.0}, {"customer": ObjectId("624b274bf7e50baefd9518d3"), "rating": 1.0, "recession": "not even worth the price"}], "categories": [ObjectId("624b2576f7e50baefd9518ce")]});
```

## Insert History
```bash
# customer 1
db.history.insert({"product": ObjectId("624b30e5f7e50baefd9518d6"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d9"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518da"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518db"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dc"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dd"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3509f7e50baefd9518de"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518db"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dc"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dd"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3509f7e50baefd9518de"), "customer": ObjectId("6249eb54f7e50baefd9518c8"), "visitedDate": ISODate()});

# customer 2
db.history.insert({"product": ObjectId("624b30e5f7e50baefd9518d6"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d9"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518da"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518db"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dc"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dd"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3509f7e50baefd9518de"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518db"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dc"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dd"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3509f7e50baefd9518de"), "customer": ObjectId("624b274af7e50baefd9518d2"), "visitedDate": ISODate()});

# customer 3
db.history.insert({"product": ObjectId("624b30e5f7e50baefd9518d6"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d9"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518da"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518db"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dc"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dd"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3509f7e50baefd9518de"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518d8"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518db"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dc"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3506f7e50baefd9518dd"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
db.history.insert({"product": ObjectId("624b3509f7e50baefd9518de"), "customer": ObjectId("624b274bf7e50baefd9518d3"), "visitedDate": ISODate()});
```

## Insert Orders
```bash
db.order.insert({"customer": ObjectId("624b274bf7e50baefd9518d3"), "products": [ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3506f7e50baefd9518dd"), ObjectId("624b3506f7e50baefd9518db")], "orderDate": ISODate()});
db.order.insert({"customer": ObjectId("624b274bf7e50baefd9518d3"), "products": [ObjectId("624b3506f7e50baefd9518d8"), ObjectId("624b3506f7e50baefd9518dc"), ObjectId("624b30e5f7e50baefd9518d6")], "orderDate": ISODate()});
db.order.insert({"customer": ObjectId("624b274bf7e50baefd9518d3"), "products": [ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3506f7e50baefd9518dd"), ObjectId("624b3506f7e50baefd9518dc"), ObjectId("624b3506f7e50baefd9518dc"), ObjectId("624b3506f7e50baefd9518db"), ObjectId("624b3506f7e50baefd9518db")], "orderDate": ISODate()});
db.order.insert({"customer": ObjectId("624b274af7e50baefd9518d2"), "products": [ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3506f7e50baefd9518dd"), ObjectId("624b3506f7e50baefd9518db")], "orderDate": ISODate()});
db.order.insert({"customer": ObjectId("624b274af7e50baefd9518d2"), "products": [ObjectId("624b3506f7e50baefd9518db"), ObjectId("624b3506f7e50baefd9518d8"), ObjectId("624b3509f7e50baefd9518de")], "orderDate": ISODate()});
db.order.insert({"customer": ObjectId("624b274af7e50baefd9518d2"), "products": [ObjectId("624b3506f7e50baefd9518db"), ObjectId("624b3506f7e50baefd9518d8"), ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3506f7e50baefd9518dd"), ObjectId("624b3506f7e50baefd9518dd"), ObjectId("624b3506f7e50baefd9518d8")], "orderDate": ISODate()});
db.order.insert({"customer": ObjectId("6249eb54f7e50baefd9518c8"), "products": [ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3509f7e50baefd9518de"), ObjectId("624b3506f7e50baefd9518dd"), ObjectId("624b3506f7e50baefd9518db")], "orderDate": ISODate()});
db.order.insert({"customer": ObjectId("6249eb54f7e50baefd9518c8"), "products": [ObjectId("624b30e5f7e50baefd9518d6"), ObjectId("624b3506f7e50baefd9518d8"), ObjectId("624b3506f7e50baefd9518d9"), ObjectId("624b3506f7e50baefd9518da"), ObjectId("624b3506f7e50baefd9518db"), ObjectId("624b3506f7e50baefd9518db"), ObjectId("624b30e5f7e50baefd9518d6"), ObjectId("624b30e5f7e50baefd9518d6"), ObjectId("624b30e5f7e50baefd9518d6"), ObjectId("624b30e5f7e50baefd9518d6"), ObjectId("624b30e5f7e50baefd9518d6")], "orderDate": ISODate()});
```