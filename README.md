# stunning-octo-engine
from typing import List, Optional
import uuid

# Entity class
class User:
    def __init__(self, name: str, email: str):
        self.id = str(uuid.uuid4())  # Unique ID
        self.name = name
        self.email = email

    def __repr__(self):
        return f"User(id='{self.id}', name='{self.name}', email='{self.email}')"


# Repository Interface
class UserRepository:
    def add(self, user: User) -> None:
        raise NotImplementedError

    def get_by_id(self, user_id: str) -> Optional[User]:
        raise NotImplementedError

    def list_all(self) -> List[User]:
        raise NotImplementedError

    def delete(self, user_id: str) -> bool:
        raise NotImplementedError


# In-memory Repository Implementation
class InMemoryUserRepository(UserRepository):
    def __init__(self):
        self._users = {}

    def add(self, user: User) -> None:
        if user.id in self._users:
            raise ValueError("User with this ID already exists.")
        self._users[user.id] = user

    def get_by_id(self, user_id: str) -> Optional[User]:
        return self._users.get(user_id)

    def list_all(self) -> List[User]:
        return list(self._users.values())

    def delete(self, user_id: str) -> bool:
        return self._users.pop(user_id, None) is not None


# Example usage
if __name__ == "__main__":
    repo = InMemoryUserRepository()

    # Add users
    user1 = User("Alice", "alice@example.com")
    user2 = User("Bob", "bob@example.com")
    repo.add(user1)
    repo.add(user2)

    # List all users
    print("All Users:", repo.list_all())

    # Get a user by ID
    print("Get User1:", repo.get_by_id(user1.id))

    # Delete a user
    repo.delete(user1.id)
    print("After Deletion:", repo.list_all())
