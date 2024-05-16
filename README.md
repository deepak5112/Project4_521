import java.util.Scanner;
import java.util.LinkedList;
import java.util.Iterator;

public class PageReplacement {
    private static final int ROWS = 100;
    private static final int COLS = 100;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of frames in the physical memory: ");
        int frames = scanner.nextInt();
        scanner.close();

        System.out.println("Number of frames entered: " + frames);

        int[][] A = new int[ROWS][COLS];

        int rowMajorPageFaults = calculatePageFaults(A, frames, true);
        int colMajorPageFaults = calculatePageFaults(A, frames, false);

        System.out.println(rowMajorPageFaults + " page faults are generated for the row-major order array-initialization.");
        System.out.println(colMajorPageFaults + " page faults are generated for the column-major order array-initialization.");
    }

    private static int calculatePageFaults(int[][] A, int frames, boolean isRowMajor) {
        LinkedList pageFrames = new LinkedList();
        int pageFaults = 0;

        for (int i = 0; i < ROWS; i++) {
            for (int j = 0; j < COLS; j++) {
                int page;
                if (isRowMajor) {
                    page = (i * COLS + j) / 100;
                } else {
                    page = (j * ROWS + i) / 100;
                }

                if (!contains(pageFrames, page)) {
                    if (pageFrames.size() == frames) {
                        pageFrames.removeFirst();
                    }
                    pageFrames.add(new Integer(page));
                    pageFaults++;
                } else {
                    remove(pageFrames, page);
                    pageFrames.add(new Integer(page));
                }
            }
        }

        return pageFaults;
    }

    private static boolean contains(LinkedList list, int page) {
        for (Iterator it = list.iterator(); it.hasNext(); ) {
            if (((Integer) it.next()).intValue() == page) {
                return true;
            }
        }
        return false;
    }

    private static void remove(LinkedList list, int page) {
        for (Iterator it = list.iterator(); it.hasNext(); ) {
            if (((Integer) it.next()).intValue() == page) {
                it.remove();
                break;
            }
        }
    }
}
# Project4_521
