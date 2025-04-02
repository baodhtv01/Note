## Các phương thức quan hệ trong Laravel Eloquent

### 1. `whereHas()`

* Chỉ lấy những users có ít nhất một post thỏa mãn điều kiện.
* Ví dụ: `users.id=1`, `posts.id=1` (thỏa) && `posts.id=2` (thỏa) => trả về `users.id=1`.

    ```php
    $users = User::whereHas('posts', function ($query) {
        $query->where('name', 'like', '%post%');
    })->get()->toArray();
    ```

### 2. `with()`

* Dùng để nạp sẵn (eager loading) một hoặc nhiều quan hệ khi truy vấn.
* Ví dụ: Lấy danh sách users kèm theo danh sách posts của họ:

    ```php
    $users = User::with('posts')->get()->toArray();
    ```

### 3. `whenLoaded()`

* Là phương thức của `JsonResource`.
* Dùng để kiểm tra xem một quan hệ đã được load hay chưa, nếu đã load thì mới thao tác tiếp.

    ```php
    $user = User::with('posts')->first(); // trả về App\Models\User
    $postsCount = $user->whenLoaded('posts', fn($posts) => $posts->count(), 0); // không gọi được vì whenLoaded là phương thức của JsonResource
    $postsCount = $user->relationLoaded('posts') ? $user->posts->count() : 0;
    ```

* Cách sử dụng đúng:

    ```php
    class UserResource extends JsonResource
    {
        public function toArray($request)
        {
            return [
                'id' => $this->id,
                'name' => $this->name,
                'posts_count' => $this->whenLoaded('posts', fn() => $this->posts->count(), 0),
            ];
        }
    }
    ```

### 4. `load()`

* Dùng để nạp thêm quan hệ sau khi đã lấy dữ liệu từ database.

    ```php
    $user = User::first();
    $user->load('posts');
    ```

### 5. `loadMissing()`

* Dùng để load quan hệ nếu nó chưa được load.

    ```php
    $user = User::first();

    // Nếu 'posts' chưa được load, load nó
    $user->loadMissing('posts');
    ```
