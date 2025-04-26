```js
  function arrayToTree(items, options = {}) {
        const {
          idKey = "id",
          parentKey = "parentId",
          childrenKey = "children",
        } = options;
        const rootItems = [];
        const itemsMap = new Map();
        items.forEach((item) => {
          itemsMap.set(item[idKey], { ...item, [childrenKey]: [] });
        });
        items.forEach((item) => {
          const node = itemsMap.get(item[idKey]);
          const parentId = item[parentKey];
          if (parentId === null || parentId === undefined) {
            rootItems.push(node);
          } else {
            const parentNode = itemsMap.get(item[parentKey]);
            if (parentNode) {
              parentNode[childrenKey].push(node);
            }
          }
        });
        return rootItems;
      }
      // 原始数据
      const employees = [
        { id: 1, name: "CEO", position: "CEO", parentId: null },
        { id: 2, name: "CTO", position: "CTO", parentId: 1 },
        { id: 3, name: "CFO", position: "CFO", parentId: 1 },
        { id: 4, name: "Dev Lead", position: "Lead", parentId: 2 },
        { id: 5, name: "Dev 1", position: "Developer", parentId: 4 },
        { id: 6, name: "Dev 2", position: "Developer", parentId: 4 },
        { id: 7, name: "Accountant", position: "Accountant", parentId: 3 },
        { id: 8, name: "AccountRoot", position: "AccountRoot", parentId: null },
      ];

      // 转换为树形结构
      const orgChart = arrayToTree(employees);
      console.log(orgChart);
```



