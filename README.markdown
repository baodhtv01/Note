# Playwright Automation with Node.js

## Mô tả ngắn

Playwright là một framework mã nguồn mở được phát triển bởi Microsoft để hỗ trợ testing và automation cho các ứng dụng web hiện đại. Nó cung cấp một API mạnh mẽ và dễ sử dụng cho việc tự động hóa trên nhiều trình duyệt như Chromium, Firefox, và WebKit, giúp các nhà phát triển kiểm tra ứng dụng của mình một cách đáng tin cậy và hiệu quả.

## Mục lục

- Giới thiệu
- Cấu hình chi tiết
- Các hàm/API chính
- Tài liệu tham khảo

## Giới thiệu

Playwright là một framework mã nguồn mở được phát triển bởi Microsoft, được thiết kế để hỗ trợ kiểm thử và tự động hóa các ứng dụng web hiện đại. Nó cung cấp một API mạnh mẽ, dễ sử dụng để tương tác với các trình duyệt như Chromium (Chrome, Edge), Firefox, và WebKit (Safari). Playwright giúp các nhà phát triển kiểm tra ứng dụng một cách đáng tin cậy, nhanh chóng và hiệu quả trên nhiều nền tảng và trình duyệt.

### So sánh với các framework khác

| Framework | Ưu điểm | Nhược điểm |
| --- | --- | --- |
| **Selenium** | Hỗ trợ nhiều ngôn ngữ, cộng đồng lớn | Tốc độ chậm, khó xử lý ứng dụng JavaScript nặng |
| **Puppeteer** | Tốt cho Chrome, API đơn giản | Chỉ hỗ trợ Chromium, hạn chế đa trình duyệt |
| **Playwright** | Đa trình duyệt, nhanh, đáng tin cậy | Tài liệu còn mới, ít plugin hơn Selenium |

Playwright được thiết kế để khắc phục các hạn chế của Selenium (tốc độ, độ tin cậy) và Puppeteer (hỗ trợ đa trình duyệt), khiến nó trở thành lựa chọn lý tưởng cho các dự án kiểm thử hiện đại.

## Cấu hình chi tiết

Playwright sử dụng file `playwright.config.js` hoặc `playwright.config.ts` để cấu hình các thiết lập kiểm thử. Dưới đây là danh sách đầy đủ các tùy chọn cấu hình chính:

| Tùy chọn | Mô tả |
| --- | --- |
| `forbidOnly` | Ngăn chặn chạy các test với `test.only` trên CI, hữu ích để tránh bỏ sót test. |
| `fullyParallel` | Chạy tất cả các test song song, tối ưu hóa thời gian thực thi. |
| `projects` | Cấu hình nhiều dự án kiểm thử cho các trình duyệt hoặc thiết bị khác nhau. |
| `reporter` | Định dạng báo cáo kết quả (ví dụ: `html`, `json`, `list`). |
| `retries` | Số lần thử lại tối đa khi test thất bại. |
| `testDir` | Thư mục chứa các file test (mặc định: `tests`). |
| `use` | Các tùy chọn chung như `baseURL`, `headless`, `trace`, v.v. |
| `webServer` | Khởi động server trong quá trình test (ví dụ: chạy `npm run start`). |
| `workers` | Số lượng worker processes chạy song song (mặc định: 3 hoặc tùy thuộc vào CPU). |
| `testIgnore` | Bỏ qua các file test theo glob patterns hoặc regex (ví dụ: `*test-assets`). |
| `testMatch` | Chọn các file test theo glob patterns (mặc định: \`.\*(test |
| `globalSetup` | File setup chạy trước tất cả các test (ví dụ: đăng nhập hoặc tạo dữ liệu). |
| `globalTeardown` | File teardown chạy sau tất cả các test (ví dụ: xóa dữ liệu hoặc đóng kết nối). |
| `outputDir` | Thư mục lưu trữ các artifact như screenshots, videos, traces (mặc định: `test-results`). |
| `timeout` | Thời gian chờ tối đa cho mỗi test, bao gồm hàm test và hooks (mặc định: 30 giây). |
| `expect.timeout` | Thời gian chờ tối đa cho các expect (mặc định: 5 giây). |
| `expect.toHaveScreenshot().maxDiffPixels` | Số lượng pixel khác biệt cho phép khi so sánh screenshot (mặc định: 10). |
| `expect.toMatchSnapshot().maxDiffPixelRatio` | Tỷ lệ pixel khác biệt cho phép trong snapshot, từ 0 đến 1 (mặc định: 0.1). |

### Ví dụ cấu hình

```javascript
// playwright.config.js
const { devices } = require('@playwright/test');

module.exports = {
  // Cấu hình nhiều dự án cho các trình duyệt
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],

  // Tùy chọn chung
  use: {
    headless: true, // Chạy ở chế độ không giao diện
    baseURL: 'http://localhost:3000', // URL cơ sở
    screenshot: 'only-on-failure', // Chụp ảnh khi thất bại
    video: 'on-first-retry', // Quay video khi thử lại lần đầu
    trace: 'on-first-retry', // Lưu trace khi thử lại
  },

  // Thời gian chờ
  timeout: 30000,
  expect: {
    timeout: 5000,
  },

  // Báo cáo HTML
  reporter: [['html', { open: 'never' }]],

  // Số lần thử lại
  retries: process.env.CI ? 2 : 0,

  // Số lượng worker
  workers: process.env.CI ? 1 : undefined,

  // Thư mục test
  testDir: './tests',

  // Bỏ qua một số file
  testIgnore: '**/*test-assets/**',

  // File setup và teardown
  globalSetup: require.resolve('./global-setup'),
  globalTeardown: require.resolve('./global-teardown'),
};
```

## Các hàm/API chính

Playwright cung cấp nhiều lớp (classes) với các phương thức (methods) để tự động hóa và kiểm thử. Dưới đây là các API chính từ các lớp phổ biến như `Page`, `Locator`, `Browser`, và `APIRequestContext`.

### Lớp Page

Lớp `Page` đại diện cho một tab trong trình duyệt và là lớp chính để tương tác với trang web. Dưới đây là các phương thức thường dùng:

- **page.goto(url)**: Điều hướng đến một URL.

  ```javascript
  await page.goto('https://example.com');
  ```

- **page.click(selector)**: Nhấp vào một phần tử (đã deprecated, nên dùng `locator.click()`).

  ```javascript
  await page.click('button.submit');
  ```

- **page.fill(selector, text)**: Điền văn bản vào ô nhập liệu (đã deprecated, nên dùng `locator.fill()`).

  ```javascript
  await page.fill('input#username', 'user123');
  ```

- **page.waitForSelector(selector)**: Chờ một phần tử xuất hiện.

  ```javascript
  await page.waitForSelector('div.success');
  ```

- **page.screenshot({ path })**: Chụp ảnh màn hình.

  ```javascript
  await page.screenshot({ path: 'screenshot.png' });
  ```

- **page.video()**: Lưu video của phiên kiểm thử.

  ```javascript
  const video = await page.video();
  await video.saveAs('video.mp4');
  ```

- **expect(page).toHaveURL()**: Kiểm tra URL của trang.

  ```javascript
  await expect(page).toHaveURL(/.*dashboard/);
  ```

- **page.locator(selector)**: Tạo một locator để tương tác với phần tử.

  ```javascript
  const button = page.locator('button.submit');
  await button.click();
  ```

- **page.getByRole(role, options)**: Tìm phần tử theo vai trò ARIA.

  ```javascript
  await page.getByRole('button', { name: 'Submit' }).click();
  ```

- **page.waitForLoadState(state)**: Chờ trạng thái tải trang (ví dụ: `load`, `domcontentloaded`).

  ```javascript
  await page.waitForLoadState('domcontentloaded');
  ```

### Lớp Locator

Lớp `Locator` cung cấp cách hiện đại để tìm và tương tác với các phần tử, hỗ trợ tự động chờ và thử lại.

- **page.locator(selector)**: Tạo một locator cho một selector.

  ```javascript
  const button = page.locator('button.submit');
  await button.click();
  ```

- **locator.waitFor()**: Chờ locator có thể tương tác.

  ```javascript
  await button.waitFor();
  ```

- **locator.isVisible()**: Kiểm tra xem locator có hiển thị không.

  ```javascript
  await expect(button).toBeVisible();
  ```

- **locator.fill(text)**: Điền văn bản vào ô nhập liệu.

  ```javascript
  await page.locator('input#username').fill('user123');
  ```

- **locator.check()**: Đánh dấu checkbox hoặc radio button.

  ```javascript
  await page.locator('input[type="checkbox"]').check();
  ```

- **locator.click()**: Nhấp vào phần tử.

  ```javascript
  await page.locator('button.submit').click();
  ```

- **expect(locator).toBeVisible()**: Kiểm tra phần tử hiển thị.

  ```javascript
  await expect(page.locator('div.alert')).toBeVisible();
  ```

### Lớp Browser

Lớp `Browser` đại diện cho một phiên trình duyệt.

- **playwright.chromium.launch()**: Khởi động trình duyệt Chromium.

  ```javascript
  const browser = await playwright.chromium.launch();
  ```

- **browser.newPage()**: Tạo một trang mới.

  ```javascript
  const page = await browser.newPage();
  ```

- **browser.close()**: Đóng trình duyệt.

  ```javascript
  await browser.close();
  ```

### Lớp APIRequestContext

Lớp `APIRequestContext` dùng để gửi các yêu cầu HTTP trực tiếp.

- **playwright.request.get(url)**: Gửi yêu cầu GET đến API.

  ```javascript
  const response = await playwright.request.get('https://api.example.com/data');
  ```

- **playwright.request.post(url, data)**: Gửi yêu cầu POST đến API.

  ```javascript
  const response = await playwright.request.post('https://api.example.com/submit', {
    data: { key: 'value' }
  });
  ```

### Các lớp và phương thức khác

Playwright còn cung cấp nhiều lớp khác như `BrowserContext`, `Frame`, `ElementHandle`, và các phương thức liên quan. Để có danh sách đầy đủ, bạn có thể tham khảo Playwright API Reference.

## Tài liệu tham khảo

- Tài liệu chính thức của Playwright
- Kho GitHub của Playwright
- Tham chiếu API của Playwright
- Cấu hình kiểm thử Playwright