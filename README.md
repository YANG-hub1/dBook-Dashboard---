dbook-dashboard/
│
├── public/
│   ├── index.html
│   └── preview-dashboard.png
│
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── BookCard.tsx
│   │   ├── BookForm.tsx
│   │   ├── CategoryBarChart.tsx
│   │   ├── CategoryPieChart.tsx
│   │   └── Header.tsx
│   ├── data/
│   │   └── books.json
│   ├── App.tsx
│   ├── index.tsx
│   ├── index.css
│   └── setupTests.ts
│
├── package.json
├── README.md
└── tsconfig.json
// src/App.tsx
import React from "react";
import BookForm from "./components/BookForm";
import BookCard from "./components/BookCard";
import CategoryPieChart from "./components/CategoryPieChart";
import CategoryBarChart from "./components/CategoryBarChart";
import Header from "./components/Header";

function App() {
  return (
    <div style={{ padding: "2rem" }}>
      <Header />
      <h1>dBook Dashboard</h1>
      <BookForm />
      <CategoryPieChart />
      <CategoryBarChart />
      <BookCard />
    </div>
  );
}

export default App;

// src/components/BookCard.tsx
import React from "react";
import { Card, Col, Row } from "antd";
import books from "../data/books.json";

const BookCard: React.FC = () => {
  return (
    <div style={{ padding: "2rem" }}>
      <Row gutter={16}>
        {books.map((book) => (
          <Col span={8} key={book.id}>
            <Card
              hoverable
              cover={<img alt={book.title} src="https://via.placeholder.com/150" />}
            >
              <Card.Meta
                title={book.title}
                description={
                  <div>
                    <p>カテゴリ: {book.category}</p>
                    <p>購読数: {book.subscription}</p>
                  </div>
                }
              />
            </Card>
          </Col>
        ))}
      </Row>
    </div>
  );
};

export default BookCard;
// src/components/BookForm.tsx
import React, { useState } from "react";
import { Form, Input, Button, Select, InputNumber, notification } from "antd";
import booksData from "../data/books.json";

const { Option } = Select;

const BookForm: React.FC = () => {
  const [books, setBooks] = useState(booksData);

  const onFinish = (values: any) => {
    const newBook = {
      id: books.length + 1,
      title: values.title,
      category: values.category,
      subscription: values.subscription,
    };
    
    setBooks([...books, newBook]);

    notification.success({
      message: "書籍が追加されました",
      description: `${values.title} がリストに追加されました。`,
    });
  };

  return (
    <div style={{ padding: "2rem" }}>
      <h2>書籍追加フォーム</h2>
      <Form onFinish={onFinish} layout="vertical">
        <Form.Item
          label="書籍タイトル"
          name="title"
          rules={[{ required: true, message: "書籍タイトルは必須です。" }]}
        >
          <Input placeholder="書籍タイトルを入力" />
        </Form.Item>

        <Form.Item
          label="カテゴリ"
          name="category"
          rules={[{ required: true, message: "カテゴリは必須です。" }]}
        >
          <Select placeholder="カテゴリを選択">
            <Option value="技術書">技術書</Option>
            <Option value="漫画">漫画</Option>
            <Option value="ビジネス書">ビジネス書</Option>
          </Select>
        </Form.Item>

        <Form.Item
          label="購読数"
          name="subscription"
          rules={[{ required: true, message: "購読数は必須です。" }]}
        >
          <InputNumber min={0} placeholder="購読数を入力" style={{ width: "100%" }} />
        </Form.Item>

        <Form.Item>
          <Button type="primary" htmlType="submit">
            書籍を追加
          </Button>
        </Form.Item>
      </Form>
    </div>
  );
};

export default BookForm;
// src/components/CategoryPieChart.tsx
import React from "react";
import { Pie } from "react-chartjs-2";
import { Chart as ChartJS, Title, Tooltip, Legend, ArcElement, CategoryScale } from "chart.js";

ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale);

const CategoryPieChart: React.FC = () => {
  const data = {
    labels: ["技術書", "漫画", "ビジネス書"],
    datasets: [
      {
        label: "書籍カテゴリ",
        data: [25, 15, 30],
        backgroundColor: ["#FF5733", "#33FF57", "#3357FF"],
      },
    ],
  };

  return <Pie data={data} />;
};

export default CategoryPieChart;
