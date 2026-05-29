Метод возвращает детальную информацию по одному заказу. Используется для просмотра статуса, состава и даты создания.
# 📘GET /{baseURL}/v1/orders/{orderId}

**Версия API:** `v1`  
**Авторизация:** `Bearer <token>`   

## 📥 Параметры запроса
| Параметр  | Расположение | Тип       | Обяз. | Описание                        | Валидация                 | Пример             |
| --------- | ------------ | --------- | ----- | ------------------------------- | ------------------------- | ------------------ |
| `orderId` | `path`       | `integer` | ✅     | Уникальный идентификатор заказа | `> 0`, без дробей/строк   | `42`               |
| `Accept`  | `header`     | `string`  | ✅     | Ожидаемый формат ответа         | Только `application/json` | `application/json` |
| `Auth`    | `header`     | `string`  | ✅     | Параметры авторизации           | JWT токен                 |                    |

**Тело запроса:** `(не используется для GET-запросов)`

## 📤 Ответы
| Код | Условие                   | Тело ответа (JSON)                                                                                  | Описание                        |
| --- | ------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------- |
| 200 | Заказ найден              | `{"id":42,"current_status_id":"SHIPPED","total_amount":1250.00,"createdAt":"2024-05-10T12:00:00Z"}` | Успешная выдача данных          |
| 400 | Неверный формат `orderId` | `{"error":"INVALID_ID","message":"Order ID must be a positive integer"}`                            | Ошибка валидации входных данных |
| 404 | Заказа нет в БД           | `{"error":"NOT_FOUND","message":"Order with id 999 does not exist"}`                                | Ресурс не найден                |
| 500 | Сбой БД/сервиса           | `{"error":"INTERNAL_ERROR","message":"Service unavailable"}`                                        | Внутренняя ошибка               |

## ⚙️ Логика обработки (Service Flow)
1. **Валидация:** Проверка формата `orderId`. При нарушении → `400`.
2. **Проверка прав (если авторизация включена):** Сверка `user_id` из токена с владельцем заказа. При несоответствии → `403 Forbidden`.
3. **Запрос к БД:** `Формируем запрос к таблице orders с условием id = orderId`
4. **Маппинг DTO:** 

| Ответ сервиса     | Параметры заполнения | Условие преобразования               |
| ----------------- | -------------------- | ------------------------------------ |
| order/            |                      |                                      |
| order/id          | orders.id            |                                      |
| order/items/      | orders.items_json    | расположить jsonb из БД внутрь блока |
| order/status      | orders.status        |                                      |
| order/total       | orders.total_amount  |                                      |
| order/createdAt   | orders.created_at    | преобразование в "дата" ISO 8601     |
| returns/[]        |                      |                                      |
| returns/id        | returns.id           |                                      |
| returns/items/    | returns.items_json   | расположить jsonb из БД внутрь блока |
| returns/status    | returns.status       |                                      |
| returns/crated_at | returns.created_at   |                                      |

5. **Возврат:** Формируется ответ `200` с `Content-Type: application/json`. При пустом результате → `404`.

## 📎 Примеры
**Запрос:**
```http
GET /{baseURL}/v1/orders/42
Accept: application/json
```

**Ответ (200 OK):**
```json
{
  "orders": {
	"id": 4,
	"items": 
	[{
		"quantity": 5, 
		"subtotal": 6000, 
		"product_name": "Кофе молотый 1кг", 
		"price_per_unit": 1200
	}, 
	{
		"quantity": 6, 
		"subtotal": 1800, 
		"product_name": "Сироп ваниль", 
		"price_per_unit": 300
	}],
	"status": "shipped",
	"total": 1250.00,
	"createdAt": "2024-05-10T12:00:00Z"
  },
  "returns":[
	  {	  
		  "id": 3,
		  "items": [{
			  "quantity": 1, 
			  "subtotal": 1200, 
			  "product_name": "Кофе молотый 1кг", 
			  "price_per_unit": 1200
		  }],
		  "status": "shipped",
		  "createdAt": "2024-05-10T12:00:00Z"
	  },
	  {
	  	  "id": 6,
	  	  "items": [{
		  	  "quantity": 1, 
		  	  "subtotal": 300, 
		  	  "product_name": "Сироп ваниль", 
		  	  "price_per_unit": 300
	  	  }],
		  "status": "shipped",
		  "createdAt": "2024-05-10T12:00:00Z"
	  }
  ]
}
```

**Ответ (404 Not Found):**

```json
{
  "error": "NOT_FOUND",
  "message": "Order with id 999 does not exist"
}
```

