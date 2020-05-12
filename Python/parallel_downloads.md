# Parallel downloads with progress bars using urlretrieve

This handy script can be used to add parallel downloads with progress bars in Python. Largely adapted from the [rich library example](https://github.com/willmcgugan/rich/blob/master/examples/downloader.py).

It uses `urlretrieve` instead of the requests library which allows you to download URLs from `ftp://`.

```
import os
from concurrent.futures import ThreadPoolExecutor
from typing import Iterable
from urllib.request import urlretrieve

from rich.progress import (
    BarColumn,
    DownloadColumn,
    TextColumn,
    TransferSpeedColumn,
    TimeRemainingColumn,
    Progress,
    TaskID,
)

progress = Progress(
    TextColumn("[bold blue]{task.fields[filename]}", justify="right"),
    BarColumn(bar_width=None),
    "[progress.percentage]{task.percentage:>3.1f}%",
    "•",
    DownloadColumn(),
    "•",
    TransferSpeedColumn(),
    "•",
    TimeRemainingColumn(),
)


class progress_bar():
    def __init__(self, task_id):
        self.task_id = task_id

    def __call__(self, block_num, block_size, total_size):
        progress.update(self.task_id, advance=1 * block_size, total=total_size)


def touch(fname, times=None):
    with open(fname, 'a'):
        os.utime(fname, times)


def copy_url(task_id: TaskID, url: str, path: str) -> None:
    """Copy data from a url to a local file."""
    urlretrieve(url, path, progress_bar(task_id))
    touch(path + ".done")


def download(urls: Iterable[str], dest_dir: str):
    """Download multuple files to the given directory."""
    with progress:
        with ThreadPoolExecutor(max_workers=4) as pool:
            for url in urls:
                filename = url.split("/")[-1]
                dest_path = os.path.join(dest_dir, filename)
                done_path = dest_path + ".done"
                if os.path.exists(done_path) is False:
                    task_id = progress.add_task("download", filename=filename)
                    pool.submit(copy_url, task_id, url, dest_path)

# Usage
download(["https://...", "ftp://..."], "download_dir"]
```