#include <iostream>
#include <string>
#include <sstream>
#include <algorithm>

using namespace std;

class LinkedList {
private:
    struct NhanVien {
        string ma, ten, ngaysinh;
        float luongCoBan, phucap;
        NhanVien* next;

        void in();
        void nhap();
        void tinhLuongThucLinh();
    };

    NhanVien* head;
public:
    LinkedList() : head(NULL) {}
    ~LinkedList();

    void themNhanVien();
    void xoaNhanVien(string ma);
    void inds();
    NhanVien* timNhanVienTheoMa(string ma);
    void timNhanVien();
    void timNhanVienTheoTen(string ten);
    void sapXep(bool byMa);
    void suaThongTinNhanVien(string ma);
    void tinhLuong();
};

void LinkedList::NhanVien::in() {
    cout << "-----------------------------------------\n";
    cout << "Ma nhan vien :" << ma << endl;
    cout << "Ten nhan vien :" << ten << endl;
    cout << "Ngay sinh :" << ngaysinh << endl;
    cout << "Luong Co ban  :" <<luongCoBan << endl;
    cout << "Phu cap  :" <<phucap << endl;
    cout << "------------------------------------------\n";
}

void LinkedList::NhanVien::nhap() {
    cout << "Nhap ma nhan vien : ";
    cin.ignore();
    getline(cin, ma);
    cout << "Nhap ten nhan vien : ";
    getline(cin, ten);
    cout << "Nhap ngay thang nam sinh : ";
    getline(cin, ngaysinh);
    cout << "Nhap luong co ban : ";
    cin>>luongCoBan;
    cout << "Nhap phu cap : ";
    cin>>phucap;
}
void LinkedList::NhanVien::tinhLuongThucLinh() {
    float luongThucLinh = phucap +luongCoBan;
    cout << "Luong thuc linh: " << luongThucLinh << endl;
}
LinkedList::~LinkedList() {
    while (head != NULL) {
        NhanVien* temp = head;
        head = head->next;
        delete temp;
    }
}
void LinkedList::themNhanVien() {
    NhanVien* newEmployee = new NhanVien;
    newEmployee->nhap();
    while (timNhanVienTheoMa(newEmployee->ma) != NULL) {
        cout << "Ma nhan vien da ton tai. Vui long nhap lai: ";
        cin.ignore();
        getline(cin, newEmployee->ma);
    }
    newEmployee->next = head;
    head = newEmployee;
}

void LinkedList::xoaNhanVien(string ma) {
    if (head == NULL) {
        cout << "Danh sach rong. Khong the xoa.\n";
        return;
    }

    if (head->ma == ma) {
        NhanVien* newHead = head->next;
        delete head;
        head = newHead;
        cout << "Da xoa nhan vien co ma " << ma << "\n";
        return;
    }

    NhanVien* current = head;
    while (current->next != NULL && current->next->ma != ma) {
        current = current->next;
    }

    if (current->next != NULL) {
        NhanVien* temp = current->next;
        current->next = current->next->next;
        delete temp;
        cout << "Da xoa nhan vien co ma " << ma << "\n";
    } else {
        cout << "Khong tim thay nhan vien co ma " << ma << "\n";
    }
}

void LinkedList::inds() {
    NhanVien* current = head;
    while (current != NULL) {
        current->in();
        current = current->next;
    }
}

LinkedList::NhanVien* LinkedList::timNhanVienTheoMa(string ma) {
    NhanVien* current = head;
    while (current != NULL) {
        if (current->ma == ma) {
            current->in();
            return current;
        }
        current = current->next;
    }
    cout << "Khong tim thay nhan vien co ma " << ma << "\n";
    return NULL;
}

void LinkedList::timNhanVien() {
    cout << "Chon phuong thuc tim kiem:\n";
    cout << "1. Theo ma nhan vien\n";
    cout << "2. Theo ten nhan vien\n";
    cout << "0. Tro ve menu chinh\n";

    int choice;
    cin >> choice;

    switch (choice) {
        case 1: {
            string ma;
            cout << "Nhap ma nhan vien can tim : ";
            cin.ignore();
            getline(cin, ma);
            timNhanVienTheoMa(ma);
            break;
        }
        case 2: {
            string ten;
            cout << "Nhap ten nhan vien can tim : ";
            cin.ignore();
            getline(cin, ten);
            timNhanVienTheoTen(ten);
            break;
        }
        case 0:
            break;
        default:
            cout << "Lua chon khong hop le. Vui long nhap lai.\n";
    }
}

void LinkedList::timNhanVienTheoTen(string ten) {
    NhanVien* current = head;
    bool found = false;
    while (current != NULL) {
        if (current->ten.find(ten) != string::npos) {
            current->in();
            found = true;
        }
        current = current->next;
    }
    if (!found)
        cout << "Khong tim thay nhan vien co ten '" << ten << "'\n";
}

void LinkedList::sapXep(bool byMa) {
    NhanVien* current = head;
    NhanVien* nextNhanVien;
    string temp;

    while (current != NULL) {
        nextNhanVien = current->next;

        while (nextNhanVien != NULL) {
            if (current->ma > nextNhanVien->ma) {
                temp =current->ma;
                if(byMa){
                    current->ma = nextNhanVien->ma;
                }else{
                    nextNhanVien->ma = temp;
            }
            nextNhanVien = nextNhanVien->next;
        }
        current = current->next;
    }

    cout << "Danh sach nhan vien sau khi sap xep theo " << ("ma") << ":\n";
    inds();
}
}

void LinkedList::suaThongTinNhanVien(string criteria) {
    NhanVien* current = head;

    cout << "Chon tieu chi sua thong tin:\n";
    cout << "1. Theo ma nhan vien\n";
    cout << "2. Theo ten nhan vien\n";
    cout << "3. Theo luong co ban \n";
    cout << "4. Theo ngay thang nam sinh\n";
    cout << "0. Tro ve menu chinh\n";

    int choice;
    cin >> choice;

    while (current != NULL) {
        bool found = false;

        switch (choice) {
            case 1:
                if (current->ma == criteria) {
                    found = true;
                }
                break;
            case 2:
                if (current->ten == criteria) {
                    found = true;
                }
                break;
            case 3:
                cout << "Nhap luong theo gio moi: ";
                cin >> current->luongCoBan;
                found = true;
                break;
            case 4:
                cout << "Nhap ngay thang nam sinh moi: ";
                cin.ignore();
                getline(cin, current->ngaysinh);
                found = true;
                break;
            case 0:
                return;
            default:
                cout << "Lua chon khong hop le. Vui long nhap lai.\n";
                return;
        }

        if (found) {
            cout << "Nhap thong tin moi cho nhan vien:\n";
            cout << "-----------------------------------------\n";
            cout << "Ma nhan vien cu: " << current->ma << endl;
            cout << "Ten nhan vien cu: " << current->ten << endl;
            cout << "Ngay sinh cu: " << current->ngaysinh << endl;
            cout << "Luong co ban cu: " << current->luongCoBan << endl;
            cout << "Phu cap cu :" << current->phucap << endl;
            cout << "------------------------------------------\n";
            
            current->nhap();
            cout << "Da sua thong tin nhan vien.\n";
            return;
        }

        current = current->next;
    }

    cout << "Khong tim thay nhan vien theo tieu chi da chon.\n";
}

void LinkedList::tinhLuong() {
    NhanVien* current = head;
    while (current != NULL) {
        current->tinhLuongThucLinh();
        current = current->next;
    }
}

int main() {
    LinkedList danhSachNhanVien;

    while (1) {
        cout << "---------------MENU---------------\n";
        cout << "1. Nhap thong tin nhan vien\n";
        cout << "2. Hien thi toan bo danh sach nhan vien\n";
        cout << "3. Tim kiem nhan vien\n";
        cout << "4. Sap xep nhan vien\n";
        cout << "5. Sua thong tin nhan vien\n";
        cout << "6. Tinh luong thuc linh\n";
        cout << "0. Thoat !\n";
        cout << "------------------------------------\n";
        cout << "Nhap lua chon: ";

        int lc;
        cin >> lc;

        switch (lc) {
            case 1:
                danhSachNhanVien.themNhanVien();
                break;
            case 2:
                danhSachNhanVien.inds();
                break;
            case 3:
                danhSachNhanVien.timNhanVien();
                break;
            case 4:
                danhSachNhanVien.sapXep(true);
                break;	
            case 5: {
                string ma;
                cout << "Nhap ma nhan vien can sua thong tin: ";
                cin.ignore();
                getline(cin, ma);
                danhSachNhanVien.suaThongTinNhanVien(ma);
                break;
            }
            case 6:
                danhSachNhanVien.tinhLuong();
                break;
            case 0:
                return 0;
            default:
                cout << "Lua chon khong hop le. Vui long nhap lai.\n";
        }
    }

    return 0;

}

