title: Typescrit/Reactjs/Antd Development Environment Setup (Jan 2021)

date: 2021-01-14 11:05:05

categories:
- 前端开发 Frontend Development

tags:
- react
- reactjs
- antd
- typescript
---

## Abstract

This article records my steps about development environment setup for Typescript,Reactjs,Antd in Jan 2021.

<!-- more -->

## Version

Software | Version
-|-
typescript  |   4.1.3
reactjs |   17.0.1
antd    |   4.10.2

## Step

### 1 Install typescript
```shell
npm install -g typescript
```

### 2 Create reactjs app using typescript
```shell
npx create-react-app my-app --template typescript
```

### 3 Install antd in development directory
```
cd my-app
npm install antd --save
```

### 4 Import antd to project

#### 4.1 Update ``src/App.tsx``

```typescript
import { Button } from 'antd';
```

#### 4.2 Update ``src/App.css``

```css
@import '~antd/dist/antd.css';
```

### 5 Run and Build

#### 5.1 Run for development
```shell
npm start
```

#### 5.2 Build for deployment
```shell
npm run build
```
Try with build

```shell
npm install -g serve
serve -s build
```
